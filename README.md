# prsh — Perl-ish Shell — Complete README

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

`prsh` is a compact interactive shell implemented in Perl with an emphasis on pleasant interactive experience rather than full POSIX-compliance. It provides:

- a colored prompt (cwd + hostname),
- a concise set of builtin commands for daily use,
- persistent history saved across sessions,
- a fallback inline editor when a full Readline implementation is absent,
- immediate `^C` feedback when idle while correctly forwarding `SIGINT` to child processes,
- basic tab completion (files + PATH executables),
- a tiny persistent config file for simple flags.

This README explains usage, installation, configuration, builtins, internals that matter to users, troubleshooting, and how to publish/update the project on GitHub.

---

## Features at a glance

- Colored prompt with working directory and hostname.
- Builtins: `cd`, `pwd`, `exit/quit`, `alias`, `unalias`, `jobs`, `systemfetch`, `uptime`, `hist`, `clearhist`, `togglehint`, `help`.
- Persistent history (`~/.prsh_history`) loaded on start and saved at exit.
- Fallback inline editor using `Term::ReadKey` when full readline backend not present:
  - Arrow-up / arrow-down history navigation
  - Tab completion for file paths and PATH executables
  - Simple backspace and printable character input
- Robust `SIGINT` handling:
  - While a child process runs: forward interrupt to child's process group.
  - While idle at prompt: print `^C` immediately and return to prompt (no delayed printing).
- Optional better experience if `Term::ReadLine::Gnu`, `IO::Pty`, `Term::ReadKey` are installed.

---

## Requirements & optional modules

**Minimum**

- Perl (5.10+ recommended)
- Typical Unix userland (`/proc` used when available)

**Optional (recommended for full UX)**

- `Term::ReadLine::Gnu` — full readline editing, history support.
- `Term::ReadKey` — required for the fallback inline editor.
- `IO::Pty` — makes interactive children behave correctly (PTY allocation).
- `Time::HiRes` — for accurate CPU sampling (usually core but sometimes separate).
- `Text::ParseWords` — used for shell-like word splitting (`shellwords`).

**Install (Debian/Ubuntu examples)**

```bash
sudo apt update
sudo apt install perl libterm-readline-gnu-perl libterm-readkey-perl libio-pty-perl
# or via CPAN/cpanminus:
cpanm Term::ReadLine::Gnu Term::ReadKey IO::Pty
