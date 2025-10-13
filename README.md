# prsh — Perl-ish Shell — Complete README

**GitHub:** https://github.com/RobertFlexx  
**Version:** BETA

A tiny, friendly interactive shell written in Perl. Colorful prompt, small set of builtins, persistent history, a fallback inline editor when a full Readline backend isn't available, immediate `^C` handling, tab completion, and a couple of convenience commands.

---

## Table of Contents

1. [Overview](#overview)  
2. [Features at a glance](#features-at-a-glance)  
3. [Requirements & optional modules](#requirements--optional-modules)  
4. [Installation (quick & manual)](#installation-quick--manual)  
5. [Running the shell](#running-the-shell)  
6. [Built-in commands (help & examples)](#built-in-commands-help--examples)  
7. [History & config files](#history--config-files)  
8. [Prompt, tab completion & fallback editor behavior](#prompt-tab-completion--fallback-editor-behavior)  
9. [Ctrl-C (SIGINT) behavior — how it works](#ctrl-c-sigint-behavior---how-it-works)  
10. [Examples and usage patterns](#examples-and-usage-patterns)  
11. [Troubleshooting & FAQ](#troubleshooting--faq)  
12. [Developing, updating repo & pushing to GitHub (git flow)](#developing-updating-repo--pushing-to-github-git-flow)  
13. [Contributing guidelines](#contributing-guidelines)  
14. [Changelog (high level)](#changelog-high-level)  
15. [License & contact](#license--contact)

---

## Overview

`prsh` is a small interactive shell with:

- colorized prompt (cwd + hostname),
- a concise set of builtins for everyday tasks,
- persistent history (`~/.prsh_history`),
- fallback inline editor (basic editing + history) when a full Readline backend isn't present,
- immediate `^C` output while idle and robust forwarding of SIGINT to children,
- tab completion for files and executables,
- a small persistent config file (`~/.prshrc`) to toggle a welcome hint.

It aims to be simple and pleasant for interactive use, not a full POSIX shell replacement.

---

## Features at a glance

- Prompt: working directory + hostname + colorful arrow
- Builtins: `cd`, `pwd`, `exit/quit`, `alias`, `unalias`, `jobs`, `systemfetch`, `uptime`, `hist`, `clearhist`, `togglehint`, `help`
- Persistent history and optional Readline integration
- Fallback non-blocking editor with:
  - arrow-up/down history navigation
  - tab completion (files + PATH executables)
  - backspace support
- Immediate `^C` printing while idle
- PTY-backed external command handling when `IO::Pty` is available

---

## Requirements & optional modules

Minimum:

- Perl (5.10+ recommended)
- Typical Unix userland

Recommended (for best experience):

- `Term::ReadLine::Gnu` — for full readline editing features
- `Term::ReadKey` — fallback editor input handling
- `IO::Pty` — proper interactive behavior for external programs
- (Perl's core modules used: `POSIX`, `Socket`, `File::Spec`, `Cwd`, `Sys::Hostname`, `Time::HiRes`, `Text::ParseWords`, `IO::Select`)

Install optional modules via your package manager or CPAN:

```bash
# example (Debian/Ubuntu)
sudo apt install libterm-readline-gnu-perl libterm-readkey-perl libio-pty-perl

# or via cpanminus
curl -L https://cpanmin.us | perl - App::cpanminus
sudo cpanm Term::ReadLine::Gnu Term::ReadKey IO::Pty
