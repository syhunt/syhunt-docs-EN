# Hunt Methods

> **Applies to:** Syhunt Hybrid 7.0.11

A *hunt method* (scan method) decides what Syhunt looks for and how aggressively it tests. **Application Scan** is the default; choose another method when you need a narrower or deeper scan.

<a id="begin"></a>
<a id="huntmethods_begin"></a>
<a id="diffstart"></a>

## Comparison

| Hunt method | CLI name | Type | Brute force | Injection | DoS | Time-consuming |
|---|---|---|---|---|---|---|
| **Application Scan** (default) | `appscan` | ▣ | Y | Y | Y | N |
| **Application Scan** (server-side focused) | `appscanss` | ▣ | Y | Y | Y | N |
| **Structure Brute Force** | `structbf` | ■ | Y (deep) | N | N | Y (very) |
| **Old & Backup Files** | `fileold` | ■ | Y | N | N | Y |
| **Fault Injection** | `faultinj` | ▣ | N | Y | Y | N |
| **OWASP Top 10** | `top10` | ▣ | N | P (Top 10) | Y | N |
| **OWASP Mobile Top 10** | `top10mob` | □ | N | P (Mobile Top 10) | N | N |
| **CWE Top 25** | `top25cwe` | ▣ | N | P (Top 25) | Y | N |
| **OWASP PHP Top 5** | `top5php` | ▣ | N | P (Top 5) | N | N |
| **Cross-Site Scripting** | `xss` | ▣ | N | P (XSS) | N | N |
| **SQL Injection** | `sqlinj` | ▣ | N | P (SQL) | N | N |
| **File Inclusion** | `fileinc` | ▣ | N | P (FI) | N | N |
| **Unvalidated Redirects** | `unvredir` | ▣ | N | P (UR) | N | N |
| **Malware Content** | `malscan` | ▣ | P (malware) | P (malware) | N | N |
| **Passive** | `passive` | ▣ | N | N | N | N |
| **Spider Only** | `spider` | ■ | N | N | N | N |
| **Complete Scan** | `complete` | ▣ | Y | Y | Y | Y (very) |
| **Complete Scan, No DoS** | `compnodos` | ▣ | Y | Y | N | Y (very) |
| **Complete Scan, Paranoid** | `comppnoid` | ▣ | Y (deep) | Y | Y | Y (very) |

**Y** = yes, **N** = no, **P** = partial (limited to the category shown).

<a id="type"></a>

**Type of testing**

- ▣ Hybrid (gray box): dynamic and code
- ■ Dynamic only (black box)
- □ Code only (white box)

<a id="timecon"></a>

**Time-consuming:** *Y* means the method runs extra checks and attack mutations, and the number of checks grows with the number of directories found while crawling.

<a id="diffend"></a>
<a id="descstart"></a>

## Method descriptions

<a id="hmas"></a>

### Application Scan

Finds flaws in custom web applications, web server software and third-party components. It crawls the site and attacks both the site structure and the applications, including fault injection tests such as XSS, SQL injection and file inclusion.

<a id="hmsbf"></a>

### Structure Brute Force

Checks for:

- Common vulnerable scripts
- Common files
- Custom (user-defined) files
- Database disclosure
- Web-based backdoors

The number of checks grows with the number of directories found while crawling.

<a id="hmfileold"></a>

### Old & Backup Files

Tries backup and old-file extensions against every item in the mapped site structure.

<a id="hmtop10"></a>

### OWASP Top 10

Scans specifically for the OWASP Top 10 (2017) risks:

1. A1: Injection
2. A2: Broken Authentication
3. A3: Sensitive Data Exposure
4. A4: XML External Entities (XXE)
5. A5: Broken Access Control
6. A6: Security Misconfiguration
7. A7: Cross-Site Scripting (XSS)
8. A8: Insecure Deserialization
9. A9: Using Components with Known Vulnerabilities
10. A10: Insufficient Logging & Monitoring

<a id="hmtop25"></a>

### CWE Top 25

Scans specifically for the [2019 CWE Top 25 Most Dangerous Software Errors](https://cwe.mitre.org/top25/archive/2019/2019_cwe_top25.html).

<a id="hmphptop5"></a>

### OWASP PHP Top 5

Scans specifically for the OWASP Top 5 PHP vulnerabilities:

1. Remote command execution
2. Cross-site scripting (XSS), including DOM XSS
3. SQL injection
4. PHP misconfiguration
5. File system attacks, including file inclusion

<a id="hmfi"></a>

### Fault Injection

Scans only for fault injection vulnerabilities. All checks that don't involve injection are disabled, and Syhunt focuses on SQL injection, XSS, file inclusion and similar flaws.

<a id="hmxss"></a>

### Cross-Site Scripting (XSS)

Scans specifically for XSS vulnerabilities, including DOM-based XSS.

<a id="hmsqli"></a>

### SQL Injection

Scans specifically for SQL and NoSQL injection vulnerabilities.

<a id="hmfileinc"></a>

### File Inclusion

Scans specifically for file inclusion and directory traversal vulnerabilities.

<a id="hmunvredir"></a>

### Unvalidated Redirects

Scans specifically for unvalidated redirect vulnerabilities.

<a id="hmmalware"></a>

### Malware Content

Scans specifically for malicious content, such as:

- Web backdoors
- Malicious content
- Hidden debug parameters

<a id="hmpassive"></a>

### Passive

Maps the site structure and reports issues without launching any attacks, for example:

- Vulnerabilities in client-side JavaScript
- Form weaknesses
- Web technology disclosure
- Insecure HTTP headers
- Outdated, vulnerable server software
- Outdated, vulnerable referenced scripts
- Suspicious HTML comments
- Source code disclosure
- Malicious content being served

<a id="hmspider"></a>

### Spider Only

Maps the site structure without testing for or reporting any vulnerability or weakness.

<a id="hmcmp"></a>

### Complete Scan

Tests for every kind of web application vulnerability using all available mutations and penetration-testing techniques, including header manipulation attacks. It can take a long time against servers with many folders and entry points.

<a id="hmcnd"></a>

### Complete Scan, No DoS

Same as Complete Scan, with denial-of-service tests disabled.

<a id="hmcpn"></a>

### Complete Scan, Paranoid

Same as Complete Scan, plus a deep structure brute force. It can take a very long time, especially on large sites. It also runs a triple-check brute force for case-sensitive servers, trying every variation of each file name (all uppercase, all lowercase, leading capitals and so on).

<a id="descend"></a>
<a id="end"></a>
