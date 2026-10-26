# Syhunt Documentation

Documentation for **Syhunt Hybrid** and its modules: Syhunt Dynamic (DAST), Syhunt Code and Mobile (SAST/MAST), Syhunt API and Syhunt Forensic.

New to Syhunt? Start with the [Welcome Guide](getting-started/welcome-guide.md), then the [Quick Start](getting-started/quick-start.md).

## Getting started

| Guide | Description |
|---|---|
| [Welcome Guide](getting-started/welcome-guide.md) | Activate your license, stay informed and find help. |
| [Quick Start](getting-started/quick-start.md) | What Syhunt does and where to go next. |
| [Download](getting-started/download.md) | Where to get the installers. |
| [Proof-of-Concept Test Plan](getting-started/poc-test-plan.md) | A step-by-step evaluation of the key features. |
| [FAQ](getting-started/faq.md) | Common questions. |

## Installation

| Guide | Description |
|---|---|
| [Installation Overview and Windows](installation/install-windows.md) | Editions, Windows setup, the Sandcat browser extension, network requirements, updating and uninstalling. |
| [Linux](installation/install-linux.md) | Step-by-step setup for Kali, Ubuntu, Debian, Fedora, RHEL-based and other distributions. |
| [macOS](installation/install-macos.md) | Setup on Apple Silicon and Intel Macs, and in Parallels Desktop. |
| [Web UI](installation/web-ui.md) | Enabling the Web UI, users, roles and password policy. |
| [Installing Wine](installation/wine.md) | Wine64 setup for the Linux and macOS (Carbon) editions. |
| [Migration Notes](installation/migration-notes.md) | What changes when upgrading between versions. |
| [Performance Tuning](installation/performance-tuning.md) | Dealing with long scans. |

## Scanning

| Guide | Description |
|---|---|
| [Dynamic Scanning (DAST)](scanning/dynamic-scanning.md) | Black-box scans of running web applications. |
| [Authenticated Scanning](scanning/authenticated-scanning.md) | Scanning areas behind a login. |
| [Source Code Scanning (SAST/MAST)](scanning/code-scanning.md) | White-box scans of web and mobile source code. |
| [Web API Scanning](scanning/api-scanning.md) | Scanning APIs from their specifications. |
| [Hunt Methods](scanning/hunt-methods.md) | Choosing a scan method. |
| [Ignore Rules](scanning/ignore-rules.md) | Suppressing findings. |
| [Scheduling Scans](scanning/scheduling.md) | Running scans automatically. |
| [Forensic Log Analysis](scanning/forensic-log-analysis.md) | Investigating attacks from web server logs. |
| [Reporting](scanning/reporting.md) | Customizing and exporting reports. |

## Integrations

| Guide | Description |
|---|---|
| [Command Line (CLI)](integrations/command-line.md) | `scanurl`, `scancode`, `scancore` and more. |
| [PowerShell](integrations/powershell.md) | `Start-DynamicScan` and `Start-CodeScan`. |
| [Web API](integrations/web-api.md) | Launching scans and getting results over HTTP. |
| [Lua API](integrations/lua-api.md) | Scripting Syhunt in Lua. |
| [GitHub](integrations/github.md) | GitHub Actions and GitHub Issues. |
| [GitLab](integrations/gitlab.md) | GitLab CI, the security dashboard and GitLab Issues. |
| [Bitbucket](integrations/bitbucket.md) | Bitbucket Pipelines. |
| [Azure DevOps](integrations/azure-devops.md) | Azure DevOps and TFS repositories. |
| [Jenkins](integrations/jenkins.md) | Jenkins pipelines and agents. |
| [Issue Trackers](integrations/issue-trackers.md) | GitHub, GitLab and Jira trackers. |
| [DefectDojo](integrations/defectdojo.md) | Sending results to DefectDojo. |
| [Faraday](integrations/faraday.md) | Sending results to Faraday. |

## Reference

| Document | Description |
|---|---|
| [Technical Specifications](reference/technical-specifications.md) | Capabilities, system requirements and glossary. |
| [Preferences](reference/preferences.md) | Every preference you can set from the CLI. |
| [Vulnerability Checks](reference/vulnerability-checks.md) | The list of web application checks. |

## Support

| Document | Description |
|---|---|
| [Reporting Bugs and Getting Support](support/reporting-bugs.md) | False positives, debug logs and support tickets. |

## About this repository

- Each page notes the product version it applies to.
- Images live in [`images/`](images/). See [images/README.md](images/README.md) for the list of files and their original sources.
- Links to pages outside this documentation point to [syhunt.com](https://www.syhunt.com/).
