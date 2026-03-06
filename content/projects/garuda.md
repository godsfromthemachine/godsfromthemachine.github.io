---
title: "Garuda"
description: "Directory tree viewer"
---

<div style="display: flex; gap: 0.75rem; align-items: center; margin-bottom: 2rem;">
  <span class="card-lang lang-zig" style="font-size: 0.85rem; padding: 0.25rem 0.75rem;">Zig</span>
  <span class="card-status status-complete" style="font-size: 0.8rem;">COMPLETE</span>
  <a href="https://github.com/godsfromthemachine/garuda" class="btn btn-outline" style="padding: 0.3rem 0.8rem; font-size: 0.75rem;">GitHub</a>
</div>

Garuda is a directory tree viewer built in Zig, replicating PowerShell's `tree` command. Named after the divine bird vehicle of Vishnu in Hindu mythology, it provides a bird's-eye view of directory structures.

## Features

- **Fast**: compiled Zig binary, instant execution
- **Colorful**: colored output for files, directories, and symlinks
- **Cross-platform**: works on Linux, macOS, and Windows
- **Lightweight**: single binary, no dependencies

## Usage

```bash
garuda              # current directory
garuda /path/to/dir # specific directory
garuda -d 3         # limit depth
```

## Status

Garuda is complete and production-ready. It's a focused utility &mdash; one tool that does one thing well.
