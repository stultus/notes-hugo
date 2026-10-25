---
title: "Modified Pre-order Tree Traversal (MPTT)"
date: 2026-10-26T03:00:00+05:30
lastmod: 2026-10-26T03:00:00+05:30
draft: false
type: "essay"
tags: ["data", "databases", "algorithms", "software-design", "postgresql", "django"]
status: "growing"
summary: "MPTT numbers a tree with entry/exit boundaries during a depth-first walk, so branch, ancestor and count queries become simple interval comparisons, at the cost of more expensive structural changes."
---

A note from first principles: what a tree is, how MPTT numbers it, why those numbers are useful, and when to use the technique. The one-line version: **record when a depth-first walk enters and leaves each node, and every subtree becomes an interval that contains the intervals of all its descendants.**

## 1. Start with a hierarchy

A **hierarchy** organizes items into levels. Folders contain other folders; a course contains modules and lessons; a shop groups products into categories.

Consider this small hierarchy:

```text
Shop
├── Fruits
│   ├── Apple
│   └── Banana
└── Drinks
    └── Tea
```

This structure is a **tree**. Each item is a **node**, and each line connects a parent to a child.

| Term | Meaning | Example |
|---|---|---|
| Root | The starting node, which has no parent | Shop |
| Parent | The node immediately above another node | Fruits is Apple's parent |
| Child | A node immediately below its parent | Apple is a child of Fruits |
| Siblings | Nodes with the same parent | Apple and Banana |
| Leaf | A node with no children | Apple, Banana, Tea |
| Ancestors | Nodes above a node along the path to the root | Fruits and Shop are Apple's ancestors |
| Descendants | All nodes underneath a node, at any depth | Fruits, Apple, Banana, Drinks, and Tea are Shop's descendants |
| Subtree | A node together with all its descendants | Fruits, Apple, and Banana |
| Depth | The number of parent–child links from the root | Shop: 0; Fruits: 1; Apple: 2 |

In the kind of tree discussed here:

- Every node except the root has exactly one parent.
- A node may have any number of children.
- Following children downward never loops back to a previous node.

A **child** is one level below a node. A **descendant** can be any number of levels below it. A node is not its own descendant.

## 2. What problem are we trying to solve?

Suppose a database stores each node and the identity of its parent. This is called an **adjacency list**.

It makes the immediate relationship easy to express: Apple belongs to Fruits, and Fruits belongs to Shop.

But applications often ask broader questions:

- What are all the descendants of Shop?
- What is the full path from Shop to Apple?
- How many nodes are underneath Fruits?

One approach is to follow the parent–child relationships whenever a question is asked. Modern databases can do this using recursive queries.

MPTT takes another approach: **store extra numbers that describe the hierarchy in advance, so many tree questions become numerical comparisons.**

The resulting representation is commonly called the **nested set model**. MPTT is the traversal technique used to assign its boundaries.

## 3. What does "pre-order traversal" mean?

**Traversal** means visiting the nodes of a tree in a systematic order.

In **pre-order traversal**:

1. Visit the current node.
2. Visit each child, completing that child's entire subtree before moving to the next child.

Choose an order for siblings. Using the displayed order in our example, the visits are:

```text
Shop → Fruits → Apple → Banana → Drinks → Tea
```

We finish the Fruits branch before starting the Drinks branch. This is a **depth-first** walk.

The word "pre" means the current node is recorded **before its descendants**.

## 4. What is modified in MPTT?

We record two events for each node:

- **Entry:** when we first reach it.
- **Exit:** when we have finished all of its descendants and are leaving its subtree.

Use one shared counter, starting at 1. Increase it after every event.

The entry number becomes the node's **left boundary**, often called `lft`. The exit number becomes its **right boundary**, often called `rgt` or `rght`.

| Step | Event | Number assigned |
|---:|---|---|
| 1 | Enter Shop | Shop.left = 1 |
| 2 | Enter Fruits | Fruits.left = 2 |
| 3 | Enter Apple | Apple.left = 3 |
| 4 | Leave Apple | Apple.right = 4 |
| 5 | Enter Banana | Banana.left = 5 |
| 6 | Leave Banana | Banana.right = 6 |
| 7 | Leave Fruits | Fruits.right = 7 |
| 8 | Enter Drinks | Drinks.left = 8 |
| 9 | Enter Tea | Tea.left = 9 |
| 10 | Leave Tea | Tea.right = 10 |
| 11 | Leave Drinks | Drinks.right = 11 |
| 12 | Leave Shop | Shop.right = 12 |

Apple has no children, so we leave it immediately after entering it. Fruits stays open until both Apple and Banana have been completed.

The numbered tree is:

```text
Shop [1, 12]
├── Fruits [2, 7]
│   ├── Apple [3, 4]
│   └── Banana [5, 6]
└── Drinks [8, 11]
    └── Tea [9, 10]
```

Each pair means **[entry, exit]**.

The names "left" and "right" refer to these boundaries. They do not mean that a node has a left child and a right child. MPTT works with any number of children.

### The numbering procedure

The following pseudocode expresses the rules. Pseudocode describes an algorithm without depending on a particular programming language.

```text
next_number = 1

visit(node):
    node.left = next_number
    next_number = next_number + 1

    for each child of node, in the chosen order:
        visit(child)

    node.right = next_number
    next_number = next_number + 1

visit(root)
```

The counter is shared across the entire walk. `visit(child)` applies the same instructions to a smaller subtree; this is called **recursion**.

For a tree with N nodes, this standard consecutive numbering uses the integers **1 through 2N**. Our six-node tree uses 1 through 12.

## 5. Why the numbers work

The key property is:

> A node's interval contains the intervals of all its descendants.

This follows directly from the walk:

1. We enter a node before entering any of its descendants.
2. We finish all those descendants before leaving the node.
3. Therefore, every descendant's entry and exit numbers fall between the node's boundaries.

For example:

```text
Fruits: [2, 7]
Apple:  [3, 4]   → entirely inside Fruits
Banana: [5, 6]   → entirely inside Fruits
Tea:    [9, 10]  → outside Fruits
```

Think of matched brackets:

```text
(Shop
    (Fruits
        (Apple)
        (Banana)
    )
    (Drinks
        (Tea)
    )
)
```

MPTT numbers the opening and closing brackets. This explains the term **nested sets**: smaller subtrees sit entirely inside larger ones.

## 6. Questions we can answer with the boundaries

The rules below compare nodes **within the same tree**.

### Is B a descendant of A?

Both of B's boundaries must be strictly inside A's boundaries:

```text
A.left < B.left
and
B.right < A.right
```

Apple is a descendant of Fruits because `2 < 3` and `4 < 7`.

These conditions find descendants at every depth. They do not prove that B is A's immediate child.

### What are a node's ancestors?

Find the nodes whose intervals strictly contain its interval.

Apple's interval is `[3, 4]`. Both Fruits `[2, 7]` and Shop `[1, 12]` contain it, so both are ancestors.

The **immediate parent** is the nearest ancestor: the containing interval with no other ancestor between it and the node. In practice, storing `parent_id` makes immediate-parent and immediate-child queries straightforward.

### Is a node a leaf?

Under the consecutive, gap-free numbering used here:

```text
right = left + 1
```

There are no descendant events between a leaf's entry and exit.

### How many descendants does a node have?

First count the numbers strictly between its boundaries:

```text
right − left − 1
```

Each descendant contributes two numbers, so:

```text
descendant_count = (right − left − 1) / 2
```

For Fruits:

```text
(7 − 2 − 1) / 2 = 2
```

The descendants are Apple and Banana.

This formula requires consecutive numbering without gaps. It counts **tree nodes**, not separate products, documents, or other records attached to those nodes.

### How do we recover pre-order?

Sort the nodes by their left boundary:

```text
Shop → Fruits → Apple → Banana → Drinks → Tea
```

The left values themselves have gaps because exit events also consume numbers.

## 7. Worked example: insert Orange under Fruits

We will add Orange as Fruits' **last child, after Banana**. Choosing a different sibling position would change which boundaries must shift.

### Step 1: Locate the insertion point

Originally:

```text
5: Enter Banana
6: Leave Banana
7: Leave Fruits
8: Enter Drinks
```

Orange must be visited before we leave Fruits. Therefore, its entry belongs at position **7**.

### Step 2: Reserve two positions

Orange needs an entry and an exit. Move every existing boundary numbered **7 or greater** forward by two:

```text
7 → 9
8 → 10
9 → 11
10 → 12
11 → 13
12 → 14
```

Positions 7 and 8 are now available. Numbers 1 through 6 remain unchanged.

### Step 3: Insert Orange

Assign Orange `[7, 8]` and set its parent to Fruits.

The local sequence becomes:

```text
5: Enter Banana
6: Leave Banana
7: Enter Orange
8: Leave Orange
9: Leave Fruits
10: Enter Drinks
```

### Step 4: Understand every change

| Node | Before | After | Explanation |
|---|---|---|---|
| Shop | [1, 12] | [1, 14] | Entry is unchanged; exit happens after two additional events. |
| Fruits | [2, 7] | [2, 9] | Entry is unchanged; Orange must be completed before exit. |
| Apple | [3, 4] | [3, 4] | Both events happen before the insertion point. |
| Banana | [5, 6] | [5, 6] | Both events happen before the insertion point. |
| Orange | — | [7, 8] | The newly added entry and exit events. |
| Drinks | [8, 11] | [10, 13] | Both events happen two positions later. |
| Tea | [9, 10] | [11, 12] | Both events happen two positions later. |

The final tree is:

```text
Shop [1, 14]
├── Fruits [2, 9]
│   ├── Apple [3, 4]
│   ├── Banana [5, 6]
│   └── Orange [7, 8]
└── Drinks [10, 13]
    └── Tea [11, 12]
```

**Why does Drinks change when Orange belongs to Fruits?** The numbers describe positions in one shared walk. Adding two earlier events shifts every later event.

- Fruits and Shop **expand**: only their right boundaries increase.
- Drinks and Tea **shift**: both boundaries increase, preserving their interval widths and contents.

Fruits now has `(9 − 2 − 1) / 2 = 3` descendants. Seven nodes use fourteen numbers.

### General rule for inserting a new last child

For this consecutive numbering scheme, within one tree:

1. Save the parent's old right boundary as `p`.
2. Increase each existing left or right boundary greater than or equal to `p` by 2.
3. Give the new node `left = p`, `right = p + 1`, and the intended parent.

The temporary gap is an intermediate step. The gap-free counting formulas apply again after insertion is complete.

In a real database, these changes must be coordinated. Concurrent structural edits require suitable locking or serialization; a transaction alone is not necessarily sufficient. Use a well-understood implementation rather than updating boundaries independently in different parts of an application.

## 8. How this fits into a database application

A database table stores rows of items and columns of properties. A practical tree table might include:

| Column | Purpose |
|---|---|
| `id` | Identifies the node independently of its position |
| `name` | Human-readable label |
| `parent_id` | Identifies its immediate parent |
| `lft`, `rgt` | Describe its subtree boundaries |
| `depth` | Records its distance from the root, if useful |
| `tree_id` | Groups nodes belonging to the same tree, if multiple trees are stored |

Column names and extra fields vary by implementation. The two boundaries are the core of the nested set representation.

Separate trees can reuse boundary numbers. If they do, comparisons must also restrict the results to the appropriate tree.

For example, after adding Orange, Fruits is `[2, 9]`. Assuming this is tree 1, the following SQL retrieves its descendants:

```sql
SELECT name
FROM categories
WHERE tree_id = 1
  AND lft > 2
  AND rgt < 9
ORDER BY lft;
```

This query returns Apple, Banana, and Orange. The strict comparisons exclude Fruits itself. Real application code would obtain the selected node's boundaries and pass them as query parameters.

The responsibilities usually divide like this:

- **Database:** stores the rows and executes the queries.
- **Backend, such as Django:** manages structural updates, permissions, and requests.
- **Frontend:** displays categories, expandable trees, or breadcrumb navigation.

The frontend can request "add Orange under Fruits" without calculating boundary numbers itself.

## 9. Benefits and costs

MPTT stores information in advance to simplify later reads. The boundaries are derived data: everything they encode can be recomputed from `parent_id` alone. Keeping both deliberately gives up the single source of truth that the [DRY Principle](/notes/dry-principle/) argues for, trading cheaper reads for writes that must keep two representations in sync.

| Operation | Consequence |
|---|---|
| Retrieve descendants or ancestors | Can use boundary comparisons instead of following links level by level |
| Count descendants | Can use arithmetic once the node's boundaries are available |
| List nodes in pre-order | Sort by the left boundary |
| Insert, remove, or move nodes | May require updating many existing boundaries |
| Edit a name or unrelated content field | Usually leaves boundaries unchanged, unless the edit also changes sibling ordering or structure |

Retrieving a large subtree still takes work: the database must find and return its rows. A single query is not a guarantee of fast execution.

The descendant-count formula counts nodes without reading every descendant. By contrast, structural updates can affect a large part of a tree, as the Orange example demonstrates.

## 10. PostgreSQL alternatives

MPTT is one way to represent a tree. PostgreSQL offers other useful approaches. Picking between them is an [Architecture Decision](/notes/architecture-decision/): it is hard to reverse once data and code depend on it, so the read/write trade-off is worth writing down.

| Representation | What is stored | Main trade-off |
|---|---|---|
| Parent relationship, or adjacency list | Each node's parent ID | Simple structural storage; branch queries explore relationships when requested |
| Materialized path, including PostgreSQL `ltree` | A path such as `shop.fruits.orange` | Supports path-based searches; moving a branch changes the paths of its descendants |
| Nested sets / MPTT | Left and right boundaries | Convenient branch queries and counts; structural changes can update many boundaries |

PostgreSQL's **recursive queries** can retrieve multiple levels of parent–child relationships within one database query. They do not require Django to make one database request per node.

With parent relationships, adding Orange can be a new row pointing to Fruits. There is no MPTT numbering to shift.

PostgreSQL's **`ltree` extension** provides a path data type, ancestor/descendant operators, and indexing support. It must be enabled; it is not an automatic behavior of an ordinary table.

For a new Django/PostgreSQL hierarchy, an indexed parent relationship is a reasonable starting point. Use ordinary queries for immediate children and evaluate recursive queries for whole branches. Compare alternatives using representative data if those queries become costly.

Django MPTT also uses database queries. The architectural choice is how to represent and maintain the hierarchy, rather than whether PostgreSQL or Django does all the work.

## 11. Where MPTT can be useful

Possible applications include:

- Product categories and subcategories.
- Documentation pages and navigation menus.
- Courses, modules, and lessons.
- Organizational departments and teams.
- Nested folders or topic collections.

Consider MPTT when the data forms a true tree, users frequently request entire branches or ancestor paths, and structural changes happen less often than reads.

For a shop, the **categories** might form the MPTT tree while products are stored separately and linked to categories. Adding a product to an existing category would then leave the category boundaries unchanged.

If items have multiple parents, or if you only need immediate children, reassess whether this representation fits. A tree-shaped interface alone does not establish a need for MPTT.

## 12. Things to remember

- **Left and right values are positions, not permanent node IDs.** They can change when the tree changes.
- **Children and descendants are different.** Interval containment finds descendants at any depth.
- **Sibling order matters.** A different traversal order produces different numbers for the same parent–child structure.
- **The standard leaf and count formulas assume gap-free numbering.** Do not apply them blindly to incomplete updates or schemes that deliberately leave gaps.
- **Tree structure and tree display are separate concerns.** A frontend can render a hierarchy stored using any of these representations.
- **Performance depends on the workload.** Measure branch reads and structural writes before choosing an optimization.

## 13. Check your understanding

Use the completed tree after Orange was added.

1. Why does Apple's pair remain `[3, 4]`?
2. Why does Shop's right boundary become 14?
3. How many descendants does Fruits `[2, 9]` have?
4. Is Tea `[11, 12]` a descendant of Fruits `[2, 9]`?
5. Is Tea a descendant of Shop `[1, 14]`? Is it an immediate child?

### Answers

1. Both Apple events occur before the insertion point, so neither position shifts.
2. Orange adds two events before we leave Shop. Seven nodes now require fourteen numbers.
3. `(9 − 2 − 1) / 2 = 3`: Apple, Banana, and Orange.
4. No. Tea's interval lies outside Fruits' interval.
5. Tea is a descendant of Shop because `1 < 11 < 12 < 14`. It is not an immediate child; its parent is Drinks.

## Related Concepts

- [Architecture Decision](/notes/architecture-decision/): choosing a tree representation is one, with a clear read/write trade-off
- [DRY Principle](/notes/dry-principle/): MPTT's boundaries are deliberately redundant with `parent_id`

## References

- [django-mptt overview](https://django-mptt.readthedocs.io/en/latest/overview.html): purpose, supported operations, and the read/update trade-off.
- [django-mptt technical details](https://django-mptt.readthedocs.io/en/latest/technical_details.html): tree IDs, depth, and concurrent structural updates.
- [Managing Hierarchical Data in MySQL — Mike Hillyer](https://mikehillyer.com/articles/managing-hierarchical-data-in-mysql/): nested sets, boundary numbering, and example queries.
- [PostgreSQL recursive queries](https://www.postgresql.org/docs/current/queries-with.html#QUERIES-WITH-RECURSIVE): exploring hierarchical relationships within a database query.
- [PostgreSQL ltree](https://www.postgresql.org/docs/current/ltree.html): hierarchical paths, operators, and indexes.
