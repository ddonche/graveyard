<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./assets/graveyard-logo-dark-nobg.png">
  <img src="./assets/graveyard-logo-light-nobg.png" alt="Graveyard" width="500">
</picture>

# Graveyard

**Graveyard** is a web framework built in [Goblin](https://github.com/ddonche/goblin-lang).

It is designed to make web application development simple, fast, and reliable while staying true to Goblin's philosophy of clarity and precision.

Unlike traditional web frameworks that assume every application needs a database server, Graveyard is built around files, shards, and request-time page assembly.

The core model is simple:

```text
forms
  → files
  → shards
  → pages
  → assembled responses
```

Application content lives as files.

Rendered content lives as shards.

Pages are assembled from shards when requested.

SQLite is used only for authentication, sessions, password hashes, reset tokens, verification tokens, and other security-related records.

Everything else is stored as Graveyard-native content.

## Features

* ⚡ Built entirely in Goblin
* 🌐 Routing for dynamic and static endpoints
* 🧩 File-based content architecture
* ⚰️ One-file shard system
* 📄 Page composition through shard assembly
* 🪦 Rebuildable indexes and headstones
* 🔐 Local authentication and sessions
* 📦 Simple configuration and deployment

## Core Concepts

### Shards

A shard is the fundamental content unit in Graveyard.

Examples include:

* Site headers
* Site footers
* Navigation blocks
* Wiki sections
* Blog content
* Forum replies
* Comments
* Profile sections
* Feed items

Each shard is stored as a single file containing metadata and rendered content.

### Pages

Pages are compositions.

A page defines which shards should appear and how they should be assembled.

In Graveyard:

```text
The page is not the unit.

The shard is the unit.
```

### Indexes

Indexes are derived lookup files.

They help Graveyard locate shards, pages, routes, authors, threads, and other relationships efficiently.

Indexes are rebuildable and are never the source of truth.

### Local Authentication

Graveyard ships with local authentication and session management.

SQLite is used only for:

* Users
* Password hashes
* Sessions
* Verification tokens
* Password reset tokens
* Rate limits

Application content remains file-based.

## Why Graveyard?

Most web frameworks assume every application requires:

* A database server
* Connection strings
* Migrations
* External infrastructure

Graveyard takes a different approach.

For many applications, content can live directly on disk as structured files.

This provides:

* Simpler deployment
* Fewer moving parts
* Easier backups
* Human-readable content
* Reduced infrastructure requirements

## Status

Graveyard is currently in early development.

Expect frequent changes and incomplete features until the first stable release.

## License

[MIT](LICENSE)
