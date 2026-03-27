---
title: "WebFinger resolves handles to identity documents in federation"
date: 2026-03-26
lastmod: 2026-03-26
draft: false
tags: ["distributed-systems", "federation", "protocols", "web-standards"]
summary: "WebFinger (RFC 7033) is a simple discovery protocol that maps a handle like @user@domain to the URL of an actor's identity document."
status: "budding"
---

**WebFinger** (RFC 7033) is the discovery mechanism that makes [federation](/notes/federation/) work. Given a handle like `@user@domain.org`, WebFinger tells you where to find that entity's full identity document.

## How It Works

Every federated server exposes a `/.well-known/webfinger` endpoint. A lookup is a simple GET request:

```
GET https://domain.org/.well-known/webfinger?resource=acct:user@domain.org
```

The response is a JSON Resource Descriptor (JRD) pointing to the [ActivityPub](/notes/activitypub/) actor document:

```json
{
  "subject": "acct:user@domain.org",
  "links": [
    {
      "rel": "self",
      "type": "application/activity+json",
      "href": "https://domain.org/ap/actor"
    }
  ]
}
```

The `rel: "self"` link with type `application/activity+json` is the pointer you need. From there, you can fetch the full actor document with inbox, outbox, and public key.

## Why the Domain Is Authoritative

The handle's domain determines which server is queried — exactly like email. To look up `user@gmail.com`, you ask Gmail's servers. This means the domain owner controls identity for all handles on that domain. If the domain goes offline, all its handles become unresolvable. This is the fundamental tradeoff of federation versus pure peer-to-peer: identity depends on domain availability.

## Related Concepts
- [ActivityPub](/notes/activitypub/) — the protocol that WebFinger enables discovery for
- [Federation](/notes/federation/) — the network model that requires cross-server discovery
