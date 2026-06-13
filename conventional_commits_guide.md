# Conventional Commits — Professional Guide

> A specification for adding human and machine-readable meaning to commit messages.

---

## Table of Contents

- [Introduction](#introduction)
- [Why Conventional Commits?](#why-conventional-commits)
- [Commit Message Structure](#commit-message-structure)
- [Commit Types](#commit-types)
- [Scope](#scope)
- [Description](#description)
- [Body](#body)
- [Footer](#footer)
- [Breaking Changes](#breaking-changes)
- [Examples](#examples)
- [Rules & Best Practices](#rules--best-practices)
- [Tooling & Integration](#tooling--integration)
- [Quick Reference](#quick-reference)

---

## Introduction

**Conventional Commits** is a lightweight convention on top of commit messages. It provides an easy set of rules for creating an explicit commit history, which makes it easier to write automated tools on top of.

This convention dovetails with [SemVer](https://semver.org/) (Semantic Versioning) by describing the features, fixes, and breaking changes made in commit messages.

---

## Why Conventional Commits?

- Automatically generate `CHANGELOG.md` files
- Automatically determine a **semantic version bump** (major, minor, patch)
- Communicate the nature of changes to teammates and stakeholders
- Trigger build and publish processes
- Make it easier for contributors to explore your commit history

---

## Commit Message Structure

Every conventional commit follows this structure:

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Rules at a Glance

| Part | Required | Description |
|------|----------|-------------|
| `type` | ✅ Yes | Category of the change |
| `scope` | ❌ Optional | Module or area affected |
| `description` | ✅ Yes | Short summary of the change |
| `body` | ❌ Optional | Detailed explanation |
| `footer` | ❌ Optional | Breaking changes, issue refs |

---

## Commit Types

| Type | Description | SemVer Impact |
|------|-------------|---------------|
| `feat` | Introduces a new feature | `MINOR` |
| `fix` | Patches a bug | `PATCH` |
| `docs` | Documentation changes only | None |
| `style` | Code style/formatting (no logic change) | None |
| `refactor` | Code restructure without feature/fix | None |
| `perf` | Performance improvement | `PATCH` |
| `test` | Adding or updating tests | None |
| `build` | Build system or dependency changes | None |
| `ci` | CI/CD configuration changes | None |
| `chore` | Other changes (e.g., tooling, configs) | None |
| `revert` | Reverts a previous commit | Depends |

---

## Scope

The **scope** provides additional contextual information. It is placed in parentheses after the type.

```
feat(auth): add OAuth2 login support
fix(api): handle null response from payment gateway
docs(readme): update installation instructions
```

Scopes should reflect the **module, package, or component** being affected. Teams should define and standardize scopes for their project.

**Examples of common scopes:**

- `auth`, `api`, `ui`, `db`, `router`, `config`, `cli`, `i18n`

---

## Description

The description is a **short, imperative summary** of the change.

### Rules

- Use the **imperative, present tense**: `add` not `added` or `adds`
- Do **not** capitalize the first letter
- Do **not** end with a period (`.`)
- Keep it under **72 characters**

```
✅ feat(cart): add item quantity update functionality
❌ feat(cart): Added item quantity update functionality.
❌ feat(cart): This commit adds item quantity update functionality
```

---

## Body

The **body** provides additional context about the change. It is separated from the description by a **blank line**.

```
fix(auth): prevent session hijacking on token refresh

The previous implementation reused the same refresh token
after each renewal, making it vulnerable to token replay attacks.
This fix invalidates the old token immediately upon use and
issues a new one with each refresh cycle.
```

### Guidelines

- Use the body to explain **what** and **why**, not **how**
- Wrap lines at **72 characters**
- Separate paragraphs with blank lines

---

## Footer

Footers appear after the body, separated by a blank line. They are used to reference issues or signal breaking changes.

```
fix(api): correct error code for unauthorized access

Closes #142
Reviewed-by: Jane Doe <jane@example.com>
```

### Common Footer Tokens

| Token | Purpose |
|-------|---------|
| `Closes #<id>` | Links and closes a GitHub/GitLab issue |
| `Fixes #<id>` | Same as Closes, commonly used for bugs |
| `Refs #<id>` | References an issue without closing it |
| `Reviewed-by:` | Credits a reviewer |
| `Co-authored-by:` | Credits a co-author |
| `BREAKING CHANGE:` | Signals a breaking change (triggers `MAJOR` bump) |

---

## Breaking Changes

A breaking change must be indicated in **one of two ways**:

### 1. Using `!` after the type/scope

```
feat(api)!: remove deprecated v1 endpoints
```

### 2. Using `BREAKING CHANGE:` in the footer

```
feat(auth): migrate to JWT-based authentication

BREAKING CHANGE: Session-based authentication has been removed.
All clients must update to use Bearer tokens in the Authorization header.
```

> **Note:** A `BREAKING CHANGE` triggers a **MAJOR** version bump in SemVer regardless of the commit type.

---

## Examples

### Simple Feature

```
feat(notifications): add email digest for weekly summaries
```

### Bug Fix with Issue Reference

```
fix(checkout): resolve price rounding error for multi-currency orders

Floating-point precision was causing incorrect totals when
converting prices between USD and EUR.

Closes #398
```

### Breaking Change with Body

```
feat(config)!: replace JSON config format with YAML

BREAKING CHANGE: The configuration file format has changed from
config.json to config.yaml. Existing JSON configs are no longer
supported. Please migrate using the provided migration script:

  npm run migrate:config
```

### Documentation Update

```
docs(contributing): add section on branch naming conventions
```

### Reverting a Commit

```
revert: feat(dashboard): add real-time analytics panel

This reverts commit a3f5c92 due to performance regression
identified in production monitoring.

Refs #501
```

---

## Rules & Best Practices

1. **One logical change per commit** — avoid mixing unrelated changes.
2. **Write in imperative mood** — `fix bug`, not `fixed bug`.
3. **Keep descriptions concise** — under 72 characters.
4. **Use the body for context** — explain the *why*, not the *how*.
5. **Always reference issues** — link relevant tickets in the footer.
6. **Flag breaking changes explicitly** — use `!` or `BREAKING CHANGE:`.
7. **Agree on scopes with your team** — consistency is key.
8. **Never amend pushed commits** — use `revert` instead.
9. **Avoid vague types like `chore` for everything** — use the most specific type.
10. **Use lowercase** — types, scopes, and descriptions should all be lowercase.

---

## Tooling & Integration

### Linting Commits

**[commitlint](https://commitlint.js.org/)** — Validates commit messages against the conventional commits spec.

```bash
# Install
npm install --save-dev @commitlint/cli @commitlint/config-conventional

# Configure (commitlint.config.js)
module.exports = { extends: ['@commitlint/config-conventional'] };
```

### Commit Helpers

**[Commitizen](https://commitizen-tools.github.io/commitizen/)** — Interactive CLI prompt to compose conventional commits.

```bash
npm install --save-dev commitizen
npx commitizen init cz-conventional-changelog --save-dev --save-exact
# Use: git cz (instead of git commit)
```

### Git Hooks

**[Husky](https://typicode.github.io/husky/)** — Enforces commit rules via Git hooks.

```bash
npm install --save-dev husky
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```

### Changelog Generation

**[standard-version](https://github.com/conventional-changelog/standard-version)** — Auto-generates changelogs and bumps version.

```bash
npm install --save-dev standard-version
npx standard-version
```

**[release-please](https://github.com/googleapis/release-please)** — GitHub Action for automated releases.

---

## Quick Reference

```
# Feature
feat(scope): short description

# Bug Fix
fix(scope): short description

# Breaking Change
feat(scope)!: short description

# With Body & Footer
type(scope): short description

Longer explanation of what changed and why.

Closes #123
BREAKING CHANGE: explanation if applicable
```

### SemVer Impact Summary

```
fix  →  PATCH  (1.0.0 → 1.0.1)
feat →  MINOR  (1.0.0 → 1.1.0)
!    →  MAJOR  (1.0.0 → 2.0.0)
```

---

## References

- [Conventional Commits Specification](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)
- [commitlint Documentation](https://commitlint.js.org/)
- [Commitizen](https://commitizen-tools.github.io/commitizen/)
- [Angular Commit Message Guidelines](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit)

---

*Last updated: March 2026 | Maintained by your documentation team*
