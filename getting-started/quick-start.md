# Quick Start

> **Applies to:** Syhunt Hybrid 7.2

Syhunt Hybrid is a multi-language web application security assessment suite. It scans for the most common web application flaws from an attacker's perspective: it injects data into web applications and analyzes their responses to determine whether the code is vulnerable to specific attacks, such as SQL injection, cross-site scripting (XSS) and many others. When requested, Syhunt also scans the application's source code for security issues.

## What can Syhunt scan?

Syhunt is designed to cover a wide range of platforms:

- **Any web server platform** (through dynamic scanning), whether it runs on Unix, Linux or Windows.
- **Devices that serve web pages**, such as routers and firewalls.
- **Web applications written in** ASP.NET, Java, Node.js, Lua, Perl, PHP, Python, Ruby and many other languages (through source code scanning).
- **Web application firewalls and intrusion detection systems** (through its evasion techniques).

<a id="proactivescanner"></a>

## A proactive approach

Web application scanners are often seen as being like antivirus software: reactive, and dependent on frequent signature updates, daily or at least weekly. That is true of traditional scanners. Since 2008, however, Syhunt has taken a proactive approach to building its checks and its hybrid scanner, focusing on common weaknesses (CWEs) rather than on individually disclosed vulnerabilities (CVEs).

| Traditional, reactive scanners | Syhunt's proactive approach |
|---|---|
| Depend heavily on regular signature updates that target CVEs (**known, disclosed vulnerabilities** in third-party applications). | Does not depend on frequent updates. Development focuses on [a list of CWEs](../reference/vulnerability-checks.md) (**common weaknesses**). CVE-specific checks ship with minor and major releases as a **complement** to the proactive checks. |
| Cannot detect undisclosed vulnerabilities in third-party applications. | Detects **both disclosed and undisclosed vulnerabilities** in third-party applications: accurately detecting a single CWE means matching hundreds of CVEs at once. |
| Miss most, if not all, vulnerabilities in your custom applications. | Is much better suited to finding vulnerabilities in your custom applications. |
| Cannot analyze source code. | Performs **hybrid analysis**: dynamic analysis complements source code analysis (DAST + SAST). |

## Next steps

| Task | Guide |
|---|---|
| Run your first dynamic (black-box) scan | [Dynamic Scanning](../scanning/dynamic-scanning.md) |
| Run your first source code (white-box) scan | [Source Code Scanning](../scanning/code-scanning.md) |
| Choose the right scan method | [Hunt Methods](../scanning/hunt-methods.md) |
| Schedule recurring scans | [Scheduling Scans](../scanning/scheduling.md) |
| Customize reports | [Reporting](../scanning/reporting.md) |
| Check what you need before a dynamic scan | [Pre-scan requirements](../scanning/dynamic-scanning.md#dynscanreq) |
| Check hardware and software requirements | [Technical Specifications](../reference/technical-specifications.md#specsbegin) |

<a id="redefineseverity_begin"></a>

## Redefining the severity of a vulnerability

If you disagree with the severity assigned to a reported vulnerability, you can set a new severity that applies to all future scans:

1. In the Web UI, open the Connectors screen (![](../images/menu.png) **Menu → Connectors**).
2. Add a connector of type **List** named `VULNSEVERITY`.
3. Enter the vulnerability's unique check ID, followed by `=` and the new severity. For example: `C-1603660271-7241=low`
4. Click **Add** to add the entry to the list.
5. Click **Save** to save the list.
