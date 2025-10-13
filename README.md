# prsh — Perl-ish Shell — Complete README (Full Details)

**GitHub:** https://github.com/RobertFlexx  
**Version:** BETA

> A tiny, friendly interactive shell written in Perl. Colorful prompt, small set of builtins, persistent history, fallback inline editor when a full Readline backend isn't available, immediate `^C` handling, tab completion, and a couple of convenience commands.

---

## Table of Contents

- [Overview](#overview)  
- [Features at a glance](#features-at-a-glance)  
- [Requirements & optional modules](#requirements--optional-modules)  
- [Installation (quick & manual)](#installation-quick--manual)  
- [Running the shell](#running-the-shell)  
- [Built-in commands (help & examples)](#built-in-commands-help--examples)  
- [History & config files](#history--config-files)  
- [Prompt, tab completion & fallback editor behavior](#prompt-tab-completion--fallback-editor-behavior)  
- [Ctrl-C (SIGINT) behavior — how it works](#ctrl-c-sigint-behavior---how-it-works)  
- [Examples and usage patterns](#examples-and-usage-patterns)  
- [Troubleshooting & FAQ](#troubleshooting--faq)  
- [Developing, updating repo & pushing to GitHub (git flow)](#developing-updating-repo--pushing-to-github-git-flow)  
- [Contributing guidelines](#contributing-guidelines)  
- [Changelog (high level)](#changelog-high-level)  
- [License & contact](#license--contact)  

---

## Overview

`prsh` is a compact interactive shell implemented in Perl. It prioritizes a pleasant interactive experience over full POSIX shell completeness. Main goals:

- fast startup,
- nice visual prompt (cwd + hostname),
- useful builtins for interactive work,
- safe and persistent history,
- a fallback inline editor for when full Readline support is absent,
- immediate and predictable `^C` behavior while idle,
- basic tab completion and PTY support for interactive children when available.

This README documents everything users and contributors need to know.

---

## Features at a glance

- Colored prompt with working directory and hostname
- Builtins: `cd`, `pwd`, `exit`/`quit`, `alias`, `unalias`, `jobs`, `systemfetch`, `uptime`, `hist`, `clearhist`, `togglehint`, `help`
- Persistent history (`~/.prsh_history`)
- Fallback inline editor using `Term::ReadKey` when full Readline backend not present:
  - Arrow-up / arrow-down to navigate history
  - Tab completion (files & PATH executables)
  - Basic editing: printable chars, backspace, enter
- Robust `SIGINT` handling:
  - Forwards `SIGINT` to children when they are running
  - Prints `^C` immediately when idle (no delayed printing)
- Optional improvements when modules installed: `Term::ReadLine::Gnu`, `IO::Pty`, `Term::ReadKey`

---

## Requirements & optional modules

### Minimum

- Perl 5.10+ (most distributions ship newer versions)
- A Unix-like environment is assumed (the code uses `/proc` when present for stats)

### Recommended / Optional for best experience

- `Term::ReadLine::Gnu` — full readline editing + proper history editing
- `Term::ReadKey` — fallback inline editor operation
- `IO::Pty` — allocate PTYs for interactive programs (vim, less, top)
- `Time::HiRes` — fine-grained sleep for CPU sampling (optional but used if available)
- `Text::ParseWords` — `shellwords()` for robust word splitting (used by script)

### Installing modules

**Debian / Ubuntu (APT)**

```bash
sudo apt update
sudo apt install libterm-readline-gnu-perl libterm-readkey-perl libio-pty-perl libtext-parsewords-perl
