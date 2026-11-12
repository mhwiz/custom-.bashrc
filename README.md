# Custom .bashrc

A customized Bash prompt for **Parrot OS** that displays useful system and network information directly in the terminal.

## Table of Contents

1. [Overview](#1-overview)
2. [Features](#2-features)
3. [Installation](#3-installation)
4. [Activating the Configuration](#4-activating-the-configuration)

---

## 1. Overview

This custom Bash prompt adds your current IP connection, as well as displaying an active ip on **tun0** for doing HTB.

The configuration can be added directly to your existing `~/.bashrc` file and activated without restarting your terminal.

## 2. Features

- **Current IP Address** — Displays the active local IP address.
- **VPN Detection** — Displays the IP address assigned to the `tun0` interface when available.
- **Network Interface Awareness** — Retrieves network information dynamically.
- **Colored Terminal Prompt** — Uses ANSI color codes to distinguish different sections.
- **Live Updates** — Refreshes the VPN interface information through `PROMPT_COMMAND`.

## 3. Installation

Open your `.bashrc` file:

```bash
nano ~/.bashrc
```

Copy the following configuration and paste it at the bottom of your `.bashrc` file:

```bash
current_ip() {
  ip -o route get 1.1.1.1 2>/dev/null |
    awk '{for (i=1; i<=NF; i++) if ($i=="src") {print $(i+1); exit}}'
}

tun0_ip() {
  ip -o -4 addr show dev tun0 2>/dev/null | awk '{print $4}' | cut -d/ -f1
}

PROMPT_COMMAND='TUN0_IP=$(tun0_ip)'

PS1='\[\e[0;31m\]┌─\[\e[0;39m\][\[\e[0;39m\]\u\[\e[1;33m\]@\[\e[1;96m\]\h\[\e[0;31m\]]─[\[\e[0;32m\]\w\[\e[0;31m\]]─[\[\e[0;35m\]$(current_ip)\[\e[0;31m\]]$( [ -n "$TUN0_IP" ] && echo "\[\e[0;36m\] [VPN: $TUN0_IP]" )\[\e[0;31m\]\n└──╼ \[\e[1;37m\]\[\e[0m\]'
```

CTRL + X, Y, and ENTER to exit and save.

## 4. Activating the Configuration

To activate the updated configuration without closing your terminal, run:

```bash
source ~/.bashrc
```

Your custom prompt should now appear in the terminal.

**Tip:** If you make changes to your `.bashrc` later, run `source ~/.bashrc` again to reload the configuration.
