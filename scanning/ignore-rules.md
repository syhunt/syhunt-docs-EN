# Ignore Rules

> **Applies to:** Syhunt Hybrid 7.2

Ignore rules stop matching vulnerabilities from being reported. They complement ignore IDs (shown at the end of each vulnerability entry in a report) when you need to suppress a broader set of findings. You can add them:

- per target, in the target's exclusion list (see [Dynamic Scanning](dynamic-scanning.md#advanced_vulnignore));
- per repository, in a `.vulnignore` file (see [Source Code Scanning](code-scanning.md#advanced_vulnignore));
- globally, for all scans (see [Global rules](#global) below).

## Syntax

A rule starts with `rule:` followed by comma-separated `parameter=value` pairs. Wrap a pair in double quotes if its value contains spaces or commas.

### Examples

| Rule | Effect |
|---|---|
| `rule:path=*,checkid=C-1603659845-9557` | Ignores any vulnerability with check ID `C-1603659845-9557`. |
| `rule:path=*,name=XSS` | Ignores any vulnerability with "XSS" in its title. |
| `rule:path=/demo/*,name=XSS` | Ignores vulnerabilities with "XSS" in the title whose path starts with `/demo/`. |
| `rule:path=*,"name=Web Technology Disclosure"` | Ignores any vulnerability with "Web Technology Disclosure" in its title. |
| `rule:path=/demo.php,lines=10` | *(SAST)* Ignores vulnerabilities on line 10 of `/demo.php`. |
| `rule:path=/demo.php,"lines=10,11"` | *(SAST)* Ignores vulnerabilities on lines 10 and 11 of `/demo.php`. |

### Parameters

| Parameter | Matches against |
|---|---|
| `path` **(required)** | The affected path. Accepts the wildcards `?` and `*`. |
| `checkid` | The vulnerability's check ID. |
| `name` | The vulnerability's title. |
| `params` | The affected parameter(s). Separate multiple parameters with commas. |
| `risk` | The vulnerability's risk: `low`, `medium`, `high` or `info`. |
| `module` | The module that found the vulnerability: `dyn` or `code`. If omitted, the rule applies to both Dynamic and Code findings. |
| `lines` | The affected source code line(s). Separate multiple lines with commas. |
| `cve` | A CVE ID among the vulnerability's CVE references. |
| `cwe` | A CWE number among the vulnerability's CWE references. |

<a id="global"></a>

## Global rules

To create rules that apply to every dynamic or code scan:

1. Open the Connectors screen (![](../images/menu.png) **Menu → Connectors**) and add a connector of type **List** named `VULNIGNORE-DAST` (for dynamic scans) or `VULNIGNORE-SAST` (for code scans).
2. Click the connector in the list to add or edit its rules.
