---
title: Nix
subtitle: A unique developer environment
date: May 2023
---

# {class=with-background-image data-background-image="assets/why-not.jpeg"}

# Challenges {class=with-background-image data-background-image="assets/challenge.jpeg"}

## Inconsistencies

::: incremental

- In developers' setups (OS, versions, tools, dependencies...)
- Across environments (local vs remote environments)

:::

## Onboarding

::: incremental

- Slow
- Prone to human error or knowledge

:::

## Dependencies and requirements

::: incremental

- application requirements need to be documented...
- and up-to-date at all time!

:::

## Context switching

Switching contexts / projects (languages, tools, versions, ...) is not trivial

# `nix` gives you powers! {class=with-background-image data-background-image="assets/superpower.jpeg"}

## declarative

> easy to initialize and maintain through versioning

## blazing fast

> using caching for dependencies

## reproducible

> as its summons an isolated environment

## reliable

> no breakages between packages

> easy rollback

> consistent states throughout upgrading

## suited for experiments

> no permanent installs

# Demo {class=with-background-image data-background-image="assets/demo.jpeg"}

Onboarding & switching context [no-brain-mode]

## From `nix` to `devenv`

```
nix + cachix + direnv = devenv
```

> A dedicated and easy-to-use wrapper of `nix-shell`

# Next steps & opportunities {class=with-background-image data-background-image="assets/opportunities.jpeg"}

::: incremental

- Build Docker images through Nix
- Use Nix in the CI (as a build system)

:::

# Resources

- https://nixos.org/guides/nix-pills/
- https://nix.dev/
- https://nixos.org/
- https://search.nixos.org/packages
