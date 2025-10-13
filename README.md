# prsh — Perl-ish Shell (Detailed README)

A tiny, friendly interactive shell written in Perl.  
Colorful prompt, small set of builtins, persistent history, a fallback inline editor when a full Readline backend isn't available, and a few nice UX touches (immediate `^C` handling, tab completion, `hist`/`clearhist`).

**GitHub:** https://github.com/RobertFlexx

---

## Table of contents

- [Overview](#overview)  
- [Features](#features)  
- [Requirements](#requirements)  
- [Optional modules (recommended)](#optional-modules-recommended)  
- [Install (quick)](#install-quick)  
- [Install (distro-specific package manager)](#install-distro-specific-package-manager)  
- [Install (CPAN)](#install-cpan)  
- [Manual install the script](#manual-install-the-script)  
- [Run & basic usage](#run--basic-usage)  
- [Builtins (detailed)](#builtins-detailed)  
- [History and config files](#history-and-config-files)  
- [Prompt, completion & fallback editor behavior](#prompt-completion--fallback-editor-behavior)  
- [Ctrl-C behavior & signal notes](#ctrl-c-behavior--signal-notes)  
- [Examples](#examples)  
- [Troubleshooting](#troubleshooting)  
- [Developing, updating repo & pushing to GitHub](#developing-updating-repo--pushing-to-github)  
- [Contributing](#contributing)  
- [License](#license)  
- [Changelog (high level)](#changelog-high-level)  
- [Contact](#contact)

---

## Overview

`prsh` is meant to be a small, pleasant interactive shell with just enough niceties to be useful:

- Colorful prompt using cwd + hostname
- Simple builtins: `cd`, `pwd`, `exit/quit`, `alias`, `unalias`, `jobs`, `systemfetch`, `uptime`, `hist`, `clearhist`, `togglehint`, `help`
- Persistent history file (`~/.prsh_history`)
- Optional `Term::ReadLine::Gnu` integration for full readline features
- Fallback "inline" editor (non-blocking read) when a full readline backend isn't available: basic editing, arrow-up/down history, tab completion
- Immediate `^C` printing while idle (no delayed printing) and forwarded `SIGINT` to child processes

---

## Features

- Colorized prompt and system information display
- `hist` and `clearhist` for history viewing & clearing
- `togglehint` to persistently hide/show the fallback Readline hint (stored in `~/.prshrc`)
- Tab completion for files and executables
- Smart handling of `Ctrl-C`:
  - forwarded to child processes when one is running
  - prints `^C` immediately while idle (no buffering/delays)
- Persistent config file `~/.prshrc` (simple `key=value`)
- History file `~/.prsh_history`

---

## Requirements

- Perl (5.10+ recommended)
- Basic Unix-y userland (POSIX `fork`, `/proc` helpful for system info)

Optional but recommended modules (see next section).

---

## Optional modules (recommended)

Install these for best experience:

- `Term::ReadLine::Gnu` — full readline editing
- `Term::ReadKey` — low-level keyboard handling for fallback editor
- `IO::Pty` — for PTY allocation so interactive programs behave correctly
- `Sys::Filesystem` gem or similar for storage info (used in Ruby variant; in Perl `df` fallback is used)

If you don't have these, the script still runs with the fallback editor.

---

## Install (quick)

Make the script executable and move it into your `$PATH`:

```bash
chmod +x prsh
# system-wide
sudo mv prsh /usr/local/bin/prsh

# or per-user
mkdir -p ~/bin
mv prsh ~/bin/
# ensure ~/bin is in your PATH
