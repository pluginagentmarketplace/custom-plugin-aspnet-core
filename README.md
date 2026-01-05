<div align="center">

<!-- Animated Typing Banner -->
<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=28&duration=3000&pause=1000&color=2E9EF7&center=true&vCenter=true&multiline=true&repeat=true&width=600&height=100&lines=Aspnet+Core+Assistant;3+Agents+%7C+3+Skills;Claude+Code+Plugin" alt="Aspnet Core Assistant" />

<br/>

<!-- Badge Row 1: Status Badges -->
[![Version](https://img.shields.io/badge/Version-1.0.0-blue?style=for-the-badge)](https://github.com/pluginagentmarketplace/custom-plugin-aspnet-core/releases)
[![License](https://img.shields.io/badge/License-Custom-yellow?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Production-brightgreen?style=for-the-badge)](#)
[![SASMP](https://img.shields.io/badge/SASMP-v1.3.0-blueviolet?style=for-the-badge)](#)

<!-- Badge Row 2: Content Badges -->
[![Agents](https://img.shields.io/badge/Agents-3-orange?style=flat-square&logo=robot)](#-agents)
[![Skills](https://img.shields.io/badge/Skills-3-purple?style=flat-square&logo=lightning)](#-skills)
[![Commands](https://img.shields.io/badge/Commands-3-green?style=flat-square&logo=terminal)](#-commands)

<br/>

<!-- Quick CTA Row -->
[📦 **Install Now**](#-quick-start) · [🤖 **Explore Agents**](#-agents) · [📖 **Documentation**](#-documentation) · [⭐ **Star this repo**](https://github.com/pluginagentmarketplace/custom-plugin-aspnet-core)

---

### What is this?

> **Aspnet Core Assistant** is a Claude Code plugin with **3 agents** and **3 skills** for aspnet core development.

</div>

---

## 📑 Table of Contents

<details>
<summary>Click to expand</summary>

- [Quick Start](#-quick-start)
- [Features](#-features)
- [Agents](#-agents)
- [Skills](#-skills)
- [Commands](#-commands)
- [Documentation](#-documentation)
- [Contributing](#-contributing)
- [License](#-license)

</details>

---

## 🚀 Quick Start

### Prerequisites

- Claude Code CLI v2.0.27+
- Active Claude subscription

### Installation (Choose One)

<details open>
<summary><strong>Option 1: From Marketplace (Recommended)</strong></summary>

```bash
# Step 1️⃣ Add the marketplace
/plugin marketplace add pluginagentmarketplace/custom-plugin-aspnet-core

# Step 2️⃣ Install the plugin
/plugin install custom-plugin-aspnet-core@pluginagentmarketplace-aspnet-core

# Step 3️⃣ Restart Claude Code
# Close and reopen your terminal/IDE
```

</details>

<details>
<summary><strong>Option 2: Local Installation</strong></summary>

```bash
# Clone the repository
git clone https://github.com/pluginagentmarketplace/custom-plugin-aspnet-core.git
cd custom-plugin-aspnet-core

# Load locally
/plugin load .

# Restart Claude Code
```

</details>

### ✅ Verify Installation

After restart, you should see these agents:

```
custom-plugin-aspnet-core:aspnet-core-architecture
custom-plugin-aspnet-core:aspnet-core-devops
custom-plugin-aspnet-core:aspnet-core-backend
```

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🤖 **3 Agents** | Specialized AI agents for aspnet core tasks |
| 🛠️ **3 Skills** | Reusable capabilities with Golden Format |
| ⌨️ **3 Commands** | Quick slash commands |
| 🔄 **SASMP v1.3.0** | Full protocol compliance |

---

## 🤖 Agents

### 3 Specialized Agents

| # | Agent | Purpose |
|---|-------|---------|
| 1 | **aspnet-core-architecture** | Expert in ASP.NET Core system design, architectural patterns |
| 2 | **aspnet-core-devops** | Expert in ASP.NET Core deployment, containerization, Azure c |
| 3 | **aspnet-core-backend** | Expert in ASP.NET Core backend development, RESTful APIs, En |

---

## 🛠️ Skills

### Available Skills

| Skill | Description | Invoke |
|-------|-------------|--------|
| `aspnet-core-fundamentals` | Master ASP.NET Core fundamentals including C#, project struc | `Skill("custom-plugin-aspnet-core:aspnet-core-fundamentals")` |
| `aspnet-core-advanced` | Master advanced ASP.NET Core development including Entity Fr | `Skill("custom-plugin-aspnet-core:aspnet-core-advanced")` |
| `aspnet-core-devops` | Master ASP.NET Core deployment, Docker, Azure cloud, CI/CD p | `Skill("custom-plugin-aspnet-core:aspnet-core-devops")` |

---

## ⌨️ Commands

| Command | Description |
|---------|-------------|
| `/learn` | ASP.NET Core Learning Path |
| `/project` | ASP.NET Core Project Templates |
| `/deploy` | ASP.NET Core Deployment Guide |

---

## 📚 Documentation

| Document | Description |
|----------|-------------|
| [CHANGELOG.md](CHANGELOG.md) | Version history |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to contribute |
| [LICENSE](LICENSE) | License information |

---

## 📁 Project Structure

<details>
<summary>Click to expand</summary>

```
custom-plugin-aspnet-core/
├── 📁 .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── 📁 agents/              # 3 agents
├── 📁 skills/              # 3 skills (Golden Format)
├── 📁 commands/            # 3 commands
├── 📁 hooks/
├── 📄 README.md
├── 📄 CHANGELOG.md
└── 📄 LICENSE
```

</details>

---

## 📅 Metadata

| Field | Value |
|-------|-------|
| **Version** | 1.0.0 |
| **Last Updated** | 2025-12-29 |
| **Status** | Production Ready |
| **SASMP** | v1.3.0 |
| **Agents** | 3 |
| **Skills** | 3 |
| **Commands** | 3 |

---

## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guide](CONTRIBUTING.md).

1. Fork the repository
2. Create your feature branch
3. Follow the Golden Format for new skills
4. Submit a pull request

---

## ⚠️ Security

> **Important:** This repository contains third-party code and dependencies.
>
> - ✅ Always review code before using in production
> - ✅ Check dependencies for known vulnerabilities
> - ✅ Follow security best practices
> - ✅ Report security issues privately via [Issues](../../issues)

---

## 📝 License

Copyright © 2025 **Dr. Umit Kacar** & **Muhsin Elcicek**

Custom License - See [LICENSE](LICENSE) for details.

---

## 👥 Contributors

<table>
<tr>
<td align="center">
<strong>Dr. Umit Kacar</strong><br/>
Senior AI Researcher & Engineer
</td>
<td align="center">
<strong>Muhsin Elcicek</strong><br/>
Senior Software Architect
</td>
</tr>
</table>

---

<div align="center">

**Made with ❤️ for the Claude Code Community**

[![GitHub](https://img.shields.io/badge/GitHub-pluginagentmarketplace-black?style=for-the-badge&logo=github)](https://github.com/pluginagentmarketplace)

</div>
