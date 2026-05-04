---
title: "The Nginx sites-available/sites-enabled Split Is a Debian Convention"
date: 2026-05-03
lastmod: 2026-05-03
draft: false
tags: ["nginx", "linux", "debian", "configuration", "operations", "conventions"]
status: "growing"
summary: "Nginx loads what its include directives reference. The two-directory split for site configs is a Debian-flavoured human workflow, not a runtime feature."
type: "note"
---

`/etc/nginx/sites-available/` and `/etc/nginx/sites-enabled/` look like an nginx feature. They aren't. Nginx has no built-in concept of "available" vs "enabled" sites — it loads whatever its `include` directives reference. On Debian and Ubuntu the default `nginx.conf` ships with one line near the bottom:

```nginx
include /etc/nginx/sites-enabled/*;
```

That single line is the entire reason `sites-enabled/` is special. Drop a config file directly into `sites-enabled/` and it loads. Skip the convention entirely and put everything in `nginx.conf` and it loads. The runtime doesn't care.

## The convention

`sites-available/` holds canonical config files — every site this server has ever defined, active or not. `sites-enabled/` holds **symlinks** back into `sites-available`. Only symlinked sites get loaded.

To enable a site: `ln -s ../sites-available/foo.conf sites-enabled/`. To disable: `rm sites-enabled/foo.conf`. The real config in `sites-available` stays put through both.

## Why the split is useful anyway

Even though nginx doesn't require it, the pattern earns its keep:

1. **Disable without deleting.** Removing a symlink is reversible and leaves no `.bak` or `.disabled` files cluttering the directory.
2. **Single source of truth.** Edits happen in `sites-available`; the symlink guarantees `sites-enabled` reflects the same content. If you keep real files in both, you eventually edit one, forget the other, and drift sets in.
3. **Tooling assumes it.** Debian's `nginx_ensite` / `nginx_dissite` helpers, plus most Ansible roles and Puppet modules, presume this layout. Break the convention and the tooling breaks with it.
4. **Audit clarity.** `sites-available` shows everything that was ever configured here; `sites-enabled` shows what is live right now. The separation pays off during incidents.

This is a [DAMP](/notes/damp-principle/) move applied to ops layout: descriptive directory names doing the work of "is this active?" so humans don't have to grep config bodies for `# disabled` comments.

## Origin and counterexamples

The split came from Apache's `a2ensite` / `a2dissite` workflow. Debian's nginx packaging copied it. RHEL and CentOS nginx packages don't ship this layout at all — they put everything in `/etc/nginx/conf.d/*.conf` and call it done. Both work; the difference is operational style.

## When the convention adds nothing

If nginx config is generated and immutable — managed by Ansible, baked into a container image, deployed through CI — the two-directory split adds no value. There's no human SSH-ing in to toggle sites, so the dance of symlinks is just a step the deployment pipeline has to fake. In that world, drop configs straight into `conf.d/` (or `sites-enabled/`, doesn't matter) and skip `sites-available` entirely.

## The transferable lesson

The general shape: **distinguish what a runtime requires from what humans find useful for managing it**. Nginx's loader has one rule (load whatever is included). The directory layout layered on top is a workflow for humans editing configs by hand. Once you see this distinction, it shows up everywhere — half of what looks like "the way the system works" is actually convention atop a much smaller core. The mistake is treating the convention as a requirement and the requirement as invisible.

The same shape underlies the Hugo theme convention of `themes/<name>/layouts/`: Hugo only requires layout files to be reachable through its lookup chain, but the theme directory pattern exists so packages can ship and be swapped cleanly. And it underlies a lot of [process is messy](/notes/process-is-messy/) — the documented process is the human convention, the actual runtime is whatever the machine ends up doing.

## Related Concepts

- [DAMP Principle](/notes/damp-principle/) — descriptive layout doing the work of state metadata, applied here to `sites-available` vs `sites-enabled`
- [Process is Messy](/notes/process-is-messy/) — the documented workflow is rarely the same as the runtime behaviour
- [Kludge](/notes/kludge/) — sometimes the convention sticks around because the original kludge worked well enough that nobody re-thought it
- [Flexibility Over Rigidity](/notes/flexibility-over-rigidity/) — knowing which conventions to keep and which to skip is itself a flex point
