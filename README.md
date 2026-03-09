# Platform Runbooks

Operational runbooks and infrastructure documentation for configuring, maintaining, and troubleshooting platform services.

This repository acts as a **central engineering knowledge base** containing step-by-step guides for infrastructure setup, networking configuration, database connectivity, data platform integrations, and operational troubleshooting.

---

# Purpose

The goal of this repository is to document repeatable operational procedures such as:

* SQL Server configuration
* Power BI Gateway setup
* Virtual Machine networking
* Firewall and port configuration
* Data synchronization
* Connectivity troubleshooting
* Developer tools configuration

These runbooks ensure infrastructure tasks can be performed consistently across environments.

---

# Repository Structure

```
docs/
│
├── infrastructure
│   ├── networking
│   ├── sql-server
│   ├── virtual-machines
│   └── security
│
├── data-platform
│   ├── powerbi
│   └── sql-sync
│
├── troubleshooting
│
└── tools
```

---

# Key Documentation Areas

## Infrastructure

Infrastructure configuration guides.

Examples:

* Enable TCP/IP in SQL Server
* Allow remote SQL connections
* Configure SQL Server ports
* Configure VM networking
* Setup firewall rules

Location:

```
docs/infrastructure/
```

---

## Data Platform

Guides related to analytics and data integrations.

Examples:

* Power BI Gateway installation
* Power BI SQL Server connections
* Data refresh troubleshooting
* Scheduled data synchronization

Location:

```
docs/data-platform/
```

---

## Troubleshooting

Common operational issues and resolutions.

Examples:

* SQL Server connection timeout
* Port connectivity testing
* Remote SQL access failures

Location:

```
docs/troubleshooting/
```

---

## Tools

Documentation for development and connectivity tools.

Examples:

* ngrok setup
* Dev tunnels
* Local environment exposure
* API debugging tools

Location:

```
docs/tools/
```

---

# Documentation Standards

All documentation should follow these conventions.

### File Naming

Use **lowercase kebab-case**

Examples:

```
enable-tcp-ip.md
powerbi-gateway-installation.md
vm-behind-nat-access.md
test-sql-connectivity.md
```

---

### Document Structure

Each runbook should include:

```
Title
Purpose
Prerequisites
Step-by-step instructions
Verification steps
Troubleshooting
```

---

### Example Structure

```
# Enable TCP/IP in SQL Server

## Purpose

Explain why TCP/IP must be enabled.

## Steps

1. Open SQL Server Configuration Manager
2. Enable TCP/IP protocol
3. Configure port
4. Restart SQL Server

## Verification

Run:

Test-NetConnection -ComputerName <server> -Port 1433

## Troubleshooting

Connection timeout
Firewall rules
SQL Server service status
```

---

# Contribution Guidelines

When adding new documentation:

1. Place files in the correct category
2. Follow naming conventions
3. Keep instructions clear and reproducible
4. Include troubleshooting notes when possible

---

# Viewing the Documentation

This documentation is built with **MkDocs** and styled with the Material theme. You can view it locally or online.

## Installation

Ensure you have Python 3.8 or higher installed, then install MkDocs and the Material theme:

```powershell
pip install mkdocs-material
```

## Running MkDocs

To serve the documentation locally and view it in your browser:

```powershell
mkdocs serve
```

The site will be available at `http://127.0.0.1:8000`

## Building Static Site

To generate static HTML files for deployment:

```powershell
mkdocs build
```

The output will be in the `site/` directory.

## Publishing to GitHub Pages

To publish the documentation to GitHub Pages:

1. **Configure your repository** for GitHub Pages:
   - Go to repository Settings → Pages
   - Set source to "Deploy from a branch"
   - Select `gh-pages` branch and root directory

2. **Deploy using MkDocs**:

```powershell
mkdocs gh-deploy
```

This command will:
- Build the static site
- Create/update the `gh-pages` branch
- Push to GitHub automatically

Your documentation will be available at: `https://<username>.github.io/<repository-name>/` or `https://motlalepule-des.github.io/platform-operations-playbook/`

3. **Optional - Automate with GitHub Actions**:

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy MkDocs

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: 3.x
      - run: pip install mkdocs-material
      - run: mkdocs gh-deploy --force
```

---

# Recommended Tools

Useful tools referenced in this repository:

| Tool                             | Purpose                       |
| -------------------------------- | ----------------------------- |
| SQL Server Configuration Manager | SQL networking configuration  |
| Power BI Gateway                 | Data refresh and connectivity |
| ngrok                            | Expose local services         |
| Dev Tunnels                      | Secure development tunneling  |
| Test-NetConnection               | Port connectivity testing     |

---

# Future Improvements

Potential future additions:

* Azure infrastructure guides
* Docker deployment runbooks
* Kubernetes operational procedures
* Monitoring and alerting documentation
* CI/CD pipeline configuration

---

# License

Internal engineering documentation.
