---

title: TTY Treatment
published: 2026-10-30
description: When you get a reverse shell during a pentest or a CTF, the shell you receive is usually very limited. It is not a real terminal, just a raw stream of input and output over a socket. This is where the TTY treatment comes in, the process of upgrading that basic shell into a fully interactive one.
image: ./logo.png
tags: [Article, Upgrading_Shell]
category: Technical Article
draft: false
---

Often, when we gain a shell, that shell isn't fully interactive and doesn't allow us to use **Ctrl + L**, **Ctrl + C**, etc...

So in this article, we're going to learn how to turn a simple shell into a fully interactive one

# Bash

```bash
script /dev/null -c bash
# Ctrl + Z
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=bash
```

Run this on your own machine. It prints your terminal’s current rows and columns **(e.g. 44 185)**

```bash
stty size
```

Run this on the target, using the numbers from the previous command. This tells the remote shell the correct window size, fixing text wrapping and preventing programs like nano, vim, etc from rendering incorrectly

```bash
stty rows <number> columns <number>
```

# ZSH

```zsh
script /dev/null -c zsh
# Ctrl + Z
stty raw -echo; fg
reset xterm
export TERM=xterm
export SHELL=zsh
```

Run this on your own machine. It prints your terminal’s current rows and columns **(e.g. 44 185)**

```bash
stty size
```

Run this on the target, using the numbers from the previous command. This tells the remote shell the correct window size, fixing text wrapping and preventing programs like nano, vim, etc from rendering incorrectly

```bash
stty rows <number> columns <number>
```

**Important:**

Zsh has its own startup files (.zshrc, .zprofile) separate from bash’s (.bashrc, .bash_profile). If the target’s zsh loads a .zshrc with aliases, prompt themes (like oh-my-zsh), or plugins, your prompt may look different or even break slightly on a minimal target — worth a one-line callout so readers aren’t confused if the prompt looks unusual after upgrading

# Step-By-Step: What Each Command Does

1. Spawn a pseudo-terminal on the target

```bash
script /dev/null -c bash
```

**script** is a Unix utility that records a terminal session. Here we’re not using it to record — we’re abusing a side effect: it allocates a real PTY (pseudo-terminal) for whatever command you give it

**/dev/null** tells it to discard the typescript log instead of saving it to a file

**-c bash** tells it which program to run inside that PTY — bash in this case

_Result:_ instead of a raw socket shell, you now have a shell running inside a proper PTY, which supports job control, signals, and terminal features.

For a zsh target, this becomes script /dev/null -c zsh, as long as zsh is actually installed on that machine (check first with which zsh or cat /etc/shells).

2. Background the shell and configure your own terminal

```bash
Ctrl+Z
```

This suspends the remote shell process and drops you back to your own local shell, in the background

```bash
stty raw -echo; fg
```

**stty raw** switches your own terminal into raw mode: keystrokes are sent directly to the program instead of being line-buffered and processed by your terminal driver. This is what makes arrow keys, Ctrl+C, and tab-completion pass through correctly

**-echo** disables local echo, since the remote shell will now handle displaying what you type (otherwise you’d see every character doubled)

**fg** brings the suspended job back to the foreground, now interacting through your raw terminal

This part is identical whether your attacking terminal runs **bash** or **zsh** — stty, job control, and fg work the same in both.

3. Reset and set the environment

```bash
reset xterm
export TERM=xterm
export SHELL=bash
```

**reset xterm** clears terminal corruption and reinitializes terminal settings, using xterm as the terminal type

**export TERM=xterm** tells programs on the target what terminal type to expect, so things like vim, nano, or less render correctly

**export SHELL=bash** sets the SHELL environment variable to match the shell actually running — change this to export SHELL=zsh if that’s what you spawned in step 1

4. Match your terminal dimensions

```bash
stty size
```

Run this on your own machine. It prints your terminal’s current rows and columns **(e.g. 44 185)**
bash

```bash
stty rows 44 columns 185
```
Run this on the target, using the numbers from the previous command. This tells the remote shell the correct window size, fixing text wrapping and preventing programs like vim from rendering incorrectly

# Bash vs Zsh — What Actually Differs

| Part                         | Bash                       | zsh                       |
| ---------------------------- | -------------------------- | ------------------------- |
| Spawning the PTY             | `script /dev/null -c bash` | `script /dev/null -c zsh` |
| `SHELL` variable             | `export SHELL=bash`        | `export SHELL=zsh`        |
| Job control (`Ctrl+Z`, `fg`) | works                      | works                     |
| `stty` commands              | works                      | works                     |
| Availability on targets      | almost always present      | rarely installed          |
