# Preferences

This page lists the main Syhunt Hybrid preferences you can set from the [command line](../integrations/command-line.md#cli_prefs). Preferences are either **global** (they affect every scan and target) or **site-specific** (they apply to one web application URL).

> [!NOTE]
> Changes don't affect scans already running; they apply to scans started afterwards. Preferences set with `scancore` are persistent and affect all future scans.

**Contents**

- [Setting values from the CLI](#values)
- [Site preferences](#prefs_site)
- [Dynamic preferences](#prefs_dynamic)
- [Code preferences](#prefs_code)
- [Forensic preferences](#prefs_forensic)
- [Other Hybrid preferences](#prefs_hybrid)
- [Tracker preferences](#prefs_tracker)

<a id="values"></a>

## Setting values from the CLI

With `scancore -prefset` and `scancore -tracker:set`, set the value according to the preference's type:

| Type | How to set it |
|---|---|
| **boolean** | `-v:true` or `-v:false` |
| **string** | `-v:value`, `-v:"my value"`, or `-vsecret` (for passwords) |
| **integer** | `-v:1000` |
| **stringlist** | `-fromfile:mylistfile.lst` to read the values from a file; `-v:[string1],[string2],[etc]` to list several strings; or `-v:value` / `-v:"my value"` for a single line |

Setting a value of the wrong type fails with an error, which protects preferences from accidental changes:

```
scancore -prefset:hybrid.report.company.logo.urldark -v:true
Error: Invalid type (provided boolean, must be string)

scancore -tg:http://127.0.0.1 -prefset:enabled -v:1
Error: Invalid type (provided integer, must be boolean)
```

Some details:

- **String lists:** in `-v:[value1],[value2],[value3]`, each bracketed value becomes a separate line, so this is stored as:
  ```
  value1
  value2
  value3
  ```
- **Spaces:** wrap values that contain spaces in double quotes, for example `-v:"my value"` or `-v:"[my value1],[my value2]"`. Don't use single quotes.
- **Secrets:** `-vsecret` asks for the value twice, masking what you type, so sensitive values such as passwords are confirmed and never appear on the command line.
- **Files:** on Windows, the file passed to `-fromfile` can be in the current folder or given with its full path. On Linux, always give the full path, for example `-fromfile:/path/to/mylistfile.lst`.

Use `-prefprint` to display the current value of any preference. For examples of setting global and site-specific preferences, see [Command Line: modifying preferences](../integrations/command-line.md#cli_prefs_exbegin).

---

<a id="prefs_site"></a>

## Site preferences

Site-specific preferences are set with `scancore -prefset` plus the `-tg` parameter, which names the target ([examples](../integrations/command-line.md#cli_prefs)). They apply only to that target, but to every DAST scan of it, however it is started: the `scanurl` command, the Web UI, the REST API, the Lua API, the PowerShell module or the classic interface.

How targets are distinguished:

- **Different paths are different targets.** For example, `scancore -tg:http://127.0.0.1 -prefset:enabled -v:true` and `scancore -tg:http://127.0.0.1/demo/ -prefset:enabled -v:true` are stored separately, even though the base URL is the same. When you run `scanurl <url>`, the preferences that match that URL's path are used.
- **Different protocols and ports are different targets.** `https://example.com`, `http://example.com` and `http://example.com:8080` each have their own preferences.
- **Need several profiles for the same URL?** Append a marker parameter, such as `?syhunt_ref=1`, `?syhunt_ref=2` and so on. The scanned application ignores it, but Syhunt treats each variant as a separate target.

| Preference | Type | Description |
|---|---|---|
| `enabled` | boolean | **true** (default): Enables site preferences for the target URL. This is a master switch.<br>**false**: Disables all site preferences, and only use default settings. |
| `name` | string | Descriptive name of the target website (for example, My company website). This name will be displayed in the user interface of Syhunt and has no impact on scans. |
| `critlevel` | string | Criticality Level. Valid values include:  **undefined** (Not Defined), **none** (Non-Critical), **low**, **medium**, **high**, **critical**. This information will be displayed in the user interface of Syhunt and has no impact on scans. Default: **undefined**. |
| `dynamic.augmented.detectoob` | boolean | **true** (default): Enables the execution of [OAST](technical-specifications.md#OAST) with Syhunt Signal (Recommended). This allows Syhunt to detect out-of-band vulnerabilities.<br>**false**: Disables all OAST testing. |
| `dynamic.augmented.scanclisource` | boolean | **true** (default): Enables the execution of SAST against client-side JavaScript with Syhunt Code<br>**false**: Disables all SAST testing over DAST. |
| `dynamic.advanced.performdos` | boolean | **true** (default): Enables the execution of DoS (Denial of Service) tests against the target web application. DoS attacks aim to disrupt the normal functioning of a web application by exploiting vulnerabilities or overwhelming its resources, thereby rendering it unavailable to legitimate users.<br>**false**: Disables all DoS testing. |
| `dynamic.advanced.performauthforce` | boolean | **true** (default): Perform authentication brute force. If enabled, Syhunt will automatically try commonly used username and passwords against all areas of the web application that ask for credentials.<br>**false**: Disables authentication brute forcing. |
| `dynamic.advanced.forcewebrootstruct` | boolean | **true** (default): Always target web root during structure brute forcing.<br>**false**: Disables targeting of web root during structure brute forcing. This means that if the target URL is `https://example.com/demo/` brute forcing will target `https://example.com/demo/*` but not `https://example.com/*` |
| `dynamic.advanced.incrementalscan` | string | Incremental Scan. Accepted string values: **auto** (default) or **disabled**. Disabling incremental scanning will result in slower future scans because the scanner won't utilize data from previous scans to expedite subsequent ones. |

### Web technologies

Syhunt Dynamic can run tests tailored to the technologies used by a web application, which speeds up testing. By default it detects these technologies automatically, which is why most of these options default to `auto`.

| Preference | Type | Description |
|---|---|---|
| `dynamic.webtech.slang` | string | The key server-side scripting language of the targeted web application. Learning in advance the programming language allows Syhunt to optimize the scan and perform only checks that are relevant to the language used by the target application. The default value is: **auto**. |

Accepted values for `dynamic.webtech.slang`:

| Value | Meaning |
|---|---|
| `auto` | Syhunt will automatically determine the language used by the target using its fingerprint capabilities. |
| `lang.asp` | The target application uses classic ASP. |
| `lang.aspx` | The target application uses ASP.NET. |
| `lang.java` | The target application uses Java or JSP (Java Server Pages). |
| `lang.kotlin` | The target application uses Kotlin. |
| `lang.ssjs` | The target application uses server-side JavaScript, such as NodeJS or similar. |
| `lang.lua` | The target application uses Lua or LuaJIT. |
| `lang.perl` | The target application uses Perl. |
| `lang.php` | The target application uses PHP. |
| `lang.python` | The target application uses Python. |
| `lang.ruby` | The target application uses Ruby or RoR. |

| Preference | Type | Description |
|---|---|---|
| `dynamic.webtech.server` | string | The web server software of the targeted web application. Learning in advance the server software allows Syhunt to optimize the scan and perform only checks that are relevant to the server used by the target application. The default value is: **auto**. |

Accepted values for `dynamic.webtech.server`:

| Value | Meaning |
|---|---|
| `auto` | Syhunt will automatically determine the server software used by the target using its fingerprint capabilities. |
| `server.apache` | The target application uses Apache. |
| `server.tomcat` | The target application uses Apache Tomcat. |
| `server.iis` | The target application uses Microsoft IIS. |
| `server.nginx` | The target application uses Nginx or OpenResty. |

| Preference | Type | Description |
|---|---|---|
| `dynamic.webtech.os` | string | The operational system of the targeted web application. Learning in advance the OS allows Syhunt to optimize the scan and perform only checks that are relevant to the OS used by the target application. The default value is: **auto**. |

Accepted values for `dynamic.webtech.os`:

| Value | Meaning |
|---|---|
| `auto` | Syhunt will automatically determine the OS of the target using its fingerprint capabilities. |
| `os.bsd` | The target application runs on top of BSD. |
| `os.linux` | The target application runs on top of a Linux distribution. |
| `os.solaris` | The target application runs on top of Solaris. |
| `os.unix` | The target application runs on top of Unix. |
| `os.windows` | The target application runs on top of Windows. |

| Preference | Type | Description |
|---|---|---|
| `dynamic.webtech.datsql` | string | The key SQL or NoSQL database used by the targeted web application. Learning in advance the database allows Syhunt to optimize the scan and perform only checks that are relevant to the database used by the target application. The default value is: **auto**. |

Accepted values for `dynamic.webtech.datsql`:

| Value | Meaning |
|---|---|
| `auto` | Syhunt will automatically determine the database of the target using its fingerprint capabilities. |
| `db.sql.access` | The target application uses Microsoft Access database. |
| `db.sql.db2` | The target application uses DB2. |
| `db.sql.informix` | The target application uses Informix. |
| `db.nosql.mongodb` | The target application uses MongoDB. |
| `db.sql.mysql` | The target application uses MariaDB or MySQL. |
| `db.sql.oracle` | The target application uses Oracle database. |
| `db.sql.postgresql` | The target application uses PortgreSQL. |
| `db.sql.sqlserver` | The target application uses Microsoft SQL Server. |
| `db.sql.sqlite` | The target application uses SQLite database. |

| Preference | Type | Description |
|---|---|---|
| `dynamic.webtech.haswaf` | string | The status of WAF presence in the targeted web application. The default value is: **auto**. |

Accepted values for `dynamic.webtech.haswaf`:

| Value | Meaning |
|---|---|
| `auto` | Syhunt will automatically determine WAF presence. |
| `inactive` | No WAF is present. |
| `active` | WAF is present. |
| `ipbypass` | WAF is present with IP bypass rule. |

| Preference | Type | Description |
|---|---|---|
| `dynamic.webtech.waf` | string | The manufacturer of the active WAF. The default value is: **auto**. |

Accepted values for `dynamic.webtech.waf`:

| Value | Meaning |
|---|---|
| `auto` | Syhunt will attempt to automatically determine the WAF manufacturer. |
| `waf.akamai` | Akamai |
| `waf.aws` | AWS |
| `waf.barracuda` | Barracuda |
| `waf.cloudflare` | CloudFlare |
| `waf.f5` | F5 |
| `waf.fortinet` | Fortinet |
| `waf.imperva` | Imperva |
| `waf.msazureappgat` | Microsoft Azure Application Gateway |
| `waf.modsecurity` | ModSecurity |
| `waf.nginx` | Nginx |
| `waf.ogasec` | OGASEC |
| `waf.radware` | Radware |
| `waf.sucuri` | Sucuri |
| `waf.unknown` | Other |

### Technology version guessing

Syhunt Dynamic can also infer which technologies and component versions a web server is running, by analyzing various aspects of the application and its responses. This lets it learn the technology stack without any input from you, for more thorough testing.

| Preference | Type | Description |
|---|---|---|
| `dynamic.webtech.guess.enabled` | boolean | **true** (default): Attempt to sense web server software versions using Syhunt HunterSense, even if banner and signatures are hidden (Recommended). This is a master switch.<br>**false**: Disables guessing of web server software. |
| `dynamic.webtech.guess.apache` | boolean | **true** (default): Sense Apache version<br>**false**: Disables guessing of Apache version. |
| `dynamic.webtech.guess.nginx` | boolean | **true** (default): Sense Nginx version<br>**false**: Disables guessing of Nginx version. |
| `dynamic.webtech.guess.php` | boolean | **true** (default): Sense PHP version<br>**false**: Disables guessing of PHP version. |
| `dynamic.webtech.guess.phusion` | boolean | **true** (default): Sense Phusion Passenger version<br>**false**: Disables guessing of Phusion Passenger version |
| `dynamic.webtech.guess.modssl` | boolean | **true** (default): Sense ModSSL version<br>**false**: Disables guessing of ModSSL version |
| `dynamic.webtech.guess.openssl` | boolean | **true** (default): Sense OpenSSL version<br>**false**: Disables guessing of OpenSSL version |

### Crawling reach

These options restrict how far Syhunt Dynamic's crawler explores. By default there are no crawling limits, so the scanner can explore a site extensively; it still applies built-in rules (Syhunt calls them *optimizations*) that guide it through the site's structure and content and speed up mapping.

| Preference | Type | Description |
|---|---|---|
| `dynamic.crawling.limitdepth` | boolean | **true**: Use depth limit. This is a master switch.<br>**false** (default): Disables depth limit. |
| `dynamic.crawling.maxdepth` | integer | Maximum depth (requires use depth limit enabled). Default: **0**., which means no limit. |
| `dynamic.crawling.limittobase` | boolean | **true**: Limit crawling to start URL path and specific allowed URLs. This is a master switch. <br>**false** (default): Disables crawling limitation to start URL path and specific allowed paths. If you enable this option, and for example, you are targeting `https://example.com/demo/` and the application links to `https://example.com/test/`, /test/ will be ignored. Because web application paths are often intertwined, this would restrict too much the scan, and because of that, this option is disabled by default. |
| `dynamic.lists.ignore.allowedpaths` | stringlist | Paths allowed to crawl (for example, /store/login.php). Default: empty list. |

The following lists don't require any master key to be enabled:

| Preference | Type | Description |
|---|---|---|
| `dynamic.lists.starturls` | stringlist | Additional Start URLs (for example, /url1.php,/url2.php). By default, Syhunt Dynamic will use the path that is part of the target URL you provide as the starting point for scanning. However, this key allows you to include additional start paths for scanning. For instance, if your target is `https://example.com/demo/` and you also want to include **/demov2/** and **/demov3/** for scanning, even if they are not directly linked within **/demo/**, adding these paths to this list ensures they will also be scanned. Default: empty list. |
| `dynamic.lists.ignore.urls` | stringlist | Paths to ignore (for example, /about1.php,/about2.php). Default: empty list. |
| `dynamic.lists.ignore.logoutpaths` | stringlist | Logout paths to ignore (for example, /logout.php). Default: empty list. Syhunt Dynamic already comes pre-configured to detect the most common logout paths. |
| `dynamic.lists.ignore.form.names` | stringlist | Forms to ignore by name (for example, search1,search2). Default: empty list. |

### Authentication

These options enable web form authentication, server authentication methods such as Basic and Bearer, or both. See [Authenticated Scanning](../scanning/authenticated-scanning.md).

| Preference | Type | Description |
|---|---|---|
| `dynamic.formauth.type` | string | Form Authentication Type. Accepted string values are: **None**, **Standard**, **Smart**, **Recorder**, **Selenium-Auto**, **Selenium-JS**, **Basic**, **Bearer**, **Digest** and **NTLM**. The default value is Standard, which means form authentication is enabled. Form authentication remains enabled as long as both the form username and password fields are not empty. |
| `dynamic.formauth.username` | string | Form username. Default: empty. |
| `dynamic.formauth.password` | string | Form password. Default: empty. |
| `dynamic.formauth.token.encrypted` | string | Token for Bearer authentication. Default: empty. |
| `dynamic.formauth.script.recorderjson.encrypted` | stringlist | [Recorder JSON for authentication](../scanning/authenticated-scanning.md#auth_recorder). Default: empty. This JSON will only be used if form authentication type is set to Recorder |
| `dynamic.formauth.script.seleniumjs.encrypted` | stringlist | [Selenium Node.JS script for authentication](../scanning/authenticated-scanning.md#auth_selenium). Default: empty. This script will only be used if form authentication type is set to Selenium-JS |
| `dynamic.lists.cookies` | stringlist | Session Cookies (for example, ck1=value;ck=value). Default: empty. |
| `dynamic.lists.cookiesforced` | stringlist | Forced Cookies (for example, ck1=value;ck=value). Default: empty. |

### Vulnerability ignore list

| Preference | Type | Description |
|---|---|---|
| `dynamic.lists.ignore.vulnsbyrules` | stringlist | Vulnerabilities to ignore (by id or rule) (for example, vuln:v1:dast-my.js-xss-748). Default: empty. |

<a id="prefs_dynamic"></a>

## Dynamic preferences

Dynamic preferences are global: they apply to every DAST scan, however it is started (the `scanurl` command, the Web UI, the REST API, the Lua API, the PowerShell module or the classic interface).

| Preference | Type | Description |
|---|---|---|
| `dynamic.checks.dos` | boolean | **true** (default): Perform DoS (Denial of Service) tests.<br>**false**: Disables DoS tests. |

### Browser emulation

| Preference | Type | Description |
|---|---|---|
| `dynamic.emulation.redirect.autofollowinstarturl` | boolean | **true** (default): Auto handle off-domain redirect in Start URL.<br>**false**: Disables off-domain redirect rendering. |
| `dynamic.emulation.mode` | string | Browser Emulation Mode. Valid options: **Chrome**, **Edge**, **Firefox**, **MSIE**, **Opera**, **Safari**. The default value is Chrome. |
| `dynamic.emulation.javascript.execution` | boolean | **true** (default): Enables JavaScript emulation.<br>**false**: Fully disables JavaScript execution and emulation. |
| `dynamic.emulation.javascript.simuser` | boolean | **true** (default): Simulate user interaction (key press, mouse click, etc).<br>**false**: Fully disables user interaction simulation. |
| `dynamic.emulation.doxhrcalls` | boolean | **true** (default): Perform XHR calls.<br>**false**: Fully disables support for XHR calls. |
| `dynamic.emulation.javascript.analyzejs` | boolean | **true** (default): Analyze JavaScript code.<br>**false**: Disables the analysis of JS code. |
| `dynamic.emulation.javascript.analyzexhr` | boolean | **true** (default): Analyze XHR calls.<br>**false**: Disables the analysis of XHR calls. |
| `dynamic.emulation.cookies.accept` | boolean | **true** (default): Accept cookies.<br>**false**: Fully disables cookie support. |
| `dynamic.emulation.cookies.maxsize` | integer | Maximum cookie size (kb). Default: **4**., which translates to 4kb. |
| `dynamic.emulation.cookies.maxnumber` | integer | Maximum number of cookies per site. Default: **50**. |
| `dynamic.emulation.autofill.forms` | boolean | **true** (default): Automatically fill web forms (name, address, phone, email, etc).<br>**false**: Disables the auto-fill of forms. |
| `dynamic.emulation.autofill.loginforms` | boolean | **true** (default): Auto fill login web forms.<br>**false**: Disables the auto-fill of login forms. |
| `dynamic.emulation.redirect.autofollow` | boolean | **true** (default): Automatically follow redirects.<br>**false**: Disables auto follow redirects. |
| `dynamic.emulation.intelparser` | boolean | **true** (default): Use intelligent HTML parsing (Handles malformed HTML).<br>**false**: Disables intelligent HTML parsing. |
| `dynamic.emulation.usenavbehavior` | boolean | **true** (default): Use browser behavior.<br>**false**: Disables browser behavior. |
| `dynamic.emulation.referer.send` | boolean | **true** (default): Send Referer.<br>**false**: Disables sending of referer. |
| `dynamic.emulation.useragent` | string | User Agent. The default value is a Chrome browser user agent. |
| `dynamic.emulation.forceuseragent` | boolean | **true** (default): Force the provided user agent in all situations.<br>**false**: Disables forcing the provided user agent in all situations. |

### Crawling reach and time limit

These global settings limit how far the crawler goes and how long each scan can run.

| Preference | Type | Description |
|---|---|---|
| `dynamic.crawling.max.linkspersite` | integer | Maximum number of links per server. Default: **10000**. |
| `dynamic.crawling.max.linksnumber` | integer | Maximum links per page. Default: **250**. |
| `dynamic.crawling.max.urlsize` | integer | Maximum URL length (bytes). Default: **16384**. |
| `dynamic.crawling.max.filesize` | integer | Maximum response size (kb). Default: **1024**. |
| `dynamic.crawling.analyze.robots` | boolean | **true** (default): Analyze robots.txt (if available)<br>**false**: Disables analysis of robots.txt file. |
| `dynamic.options.logout.prevent` | boolean | **true** (default): Enabling this option prevents Syhunt Dynamic from inadvertently visiting the logout page of the web application during crawling or testing. This helps avoid unintentional session termination.<br>**false**: Disables logout prevention mechanisms. |
| `dynamic.options.logout.detect` | boolean | **true** (default): Relogin after premature session termination when needed.<br>**false**: Disables relogin. |

| Preference | Type | Description |
|---|---|---|
| `syhunt.dynamic.options.timelimit.enabled` | boolean | **false** (default): Do not use any time limit.<br>**true**: Enable time limit. This is a master switch |
| `syhunt.dynamic.options.timelimit.value` | integer | The scan time limit parameter determines the maximum duration for a scan. Default: empty., indicating no limit. Accepted time limits include formats like: **1d** for one day, **3h** for three hours, **2h30m** for two hours and thirty minutes, or **50m** for fifty minutes. Once this maximum duration is reached, the scan automatically cancels itself |

### Delay

Sets a delay, in milliseconds, between requests. We advise against it for DAST, which typically involves millions of requests: any delay can make scans take much longer.

| Preference | Type | Description |
|---|---|---|
| `dynamic.options.delay.enabled` | boolean | **false** (default): Do not use delay between requests<br>**true**: Enables delay between requests. This is a master switch. |
| `dynamic.options.delay.value` | integer | Delay (ms). Default: **3000**. |
| `dynamic.options.delay.userandom` | boolean | **false** (default): Do not use random delay.<br>**true**: Enables random delay between requests |

### Evasion

| Preference | Type | Description |
|---|---|---|
| `dynamic.evasion.detection.honeypot` | boolean | **true** (default): Detect if the target application is a honeypot.<br>**false**: Disables detection of honeypot. |
| `dynamic.evasion.detection.waf` | boolean | **true** (default): Attempts to detect if the application is behind WAF (web application firewall)<br>**false**: Disables detection of WAF. |
| `dynamic.evasion.reqdistrand` | boolean | **true**: Distribute requests randomly during crawling (experimental)<br>**false** (default): Disables random distribution of requests. |
| `dynamic.evasion.evadeflt` | boolean | **true** (default): Use filter evasion<br>**false**: Disables filter evasion. This is a master switch. |
| `dynamic.checks.sub.xssev` | boolean | **true** (default): Evasion Technique: Common XSS filter evasion techniques.<br>**false**: Disables Common XSS filter evasion techniques. |
| `dynamic.checks.sub.utf8dev` | boolean | **true** (default): Evasion Technique: UTF-8 Decode.<br>**false**: Disables UTF-8 Decode evasion technique. |

### Protocol

The defaults below are recommended: they are optimized for handling web traffic efficiently and give the best scan performance.

| Preference | Type | Description |
|---|---|---|
| `dynamic.protocol.version` | string | Protocol Version. Accepts: HTTP/1.1 and HTTP/1.0. Default: **HTTP/1.1**. This is also the recommended value. |
| `dynamic.protocol.ssl.type` | string | SSL Type. Accepts: all, SSLv2, SSLv3, TLSv1, TLSv1_1, TLSv1_2, TLSv1_3, SSHv2. Default: **all**. This is also the recommended value, since it works with all supported SSL types instead of restricting to a specific SSL type. |
| `dynamic.protocol.keepalive` | boolean | **true** (default): Enable Keep-Alive support (recommended).<br>**false**: Disables Keep-Alive support. This negatively impacts the scan speed since Keep-Alive speeds up connections. |
| `dynamic.protocol.usegzip` | boolean | **true** (default): Enable GZIP compression support (recommended).<br>**false**: Disables GZIP compression support. This negatively impacts the scan speed since GZIP compression speeds up connections. |
| `dynamic.protocol.mknocache` | boolean | **true** (default): Make no-cache requests.<br>**false**: Disables no-cache requests. |
| `dynamic.protocol.timeout.value` | integer | Timeout (ms). Default: **10000**. |
| `dynamic.protocol.retries` | integer | Number of retries after timeout. Default: **2**. |
| `syhunt.dynamic.options.augmented.jndiserver` | string | JNDIExploit Server (Host without port) for Log4Shell detection. Default: empty. |
| `dynamic.lists.protocol.customhdr` | stringlist | Custom Request Headers (for example, X-Custom:AValue). Default: empty. |

### Vulnerability ignore list

Each DAST vulnerability entry in a report ends with its ignore ID, the simplest and recommended way to suppress a finding. For broader scenarios, use [ignore rules](../scanning/ignore-rules.md).

| Preference | Type | Description |
|---|---|---|
| `dynamic.lists.ignore.vulnsbyrules` | stringlist | Vulnerabilities to ignore (by rule or id). Default: empty list. |

---
<a id="prefs_code"></a>

## Code preferences

Code preferences are global: they apply to every SAST scan, however it is started (the `scancode` command, SAST within DAST, the Web UI, the REST API, the Lua API, the PowerShell module or the classic interface).

| Preference | Type | Description |
|---|---|---|
| `code.checks.inflt` | boolean | **true** (default): Analyze input filtering in application source code. This is crucial for eliminating false positives, as it ensures proper handling of user input, reducing the likelihood of incorrect detections. <br>**false**: Disables analysis of input filtering. |
| `code.options.target.tfsver` | string | TFS Version Compatibility. Accepts: latest, 2017, 2015, 2013, 2012, 2010. Default: **latest**., which means 2018 or higher. |

### Vulnerability ignore list

Each SAST vulnerability entry in a report ends with its ignore ID, the simplest and recommended way to suppress a finding; for broader scenarios, use [ignore rules](../scanning/ignore-rules.md). For SAST, rather than `code.lists.ignore.vulnsbyrules`, the recommended practice is a [`.vulnignore` file](../scanning/code-scanning.md#advanced_vulnignore) in the scanned repository.

| Preference | Type | Description |
|---|---|---|
| `code.lists.ignore.vulnsbyrules` | stringlist | Vulnerabilities to ignore (by rule or id). Default: empty list. |

### Time limit

| Preference | Type | Description |
|---|---|---|
| `syhunt.code.options.timelimit.enabled` | boolean | **false** (default): Do not use any time limit.<br>**true**: Enable time limit. This is a master switch |
| `syhunt.code.options.timelimit.value` | integer | The scan time limit parameter determines the maximum duration for a scan. Default: empty., indicating no limit. Accepted time limits include formats like: **1d** for one day, **3h** for three hours, **2h30m** for two hours and thirty minutes, or **50m** for fifty minutes. Once this maximum duration is reached, the scan automatically cancels itself |

---

<a id="prefs_forensic"></a>

## Forensic preferences

Forensic preferences are global: they apply to every FAST scan, whether started with the `scanlog` command or the classic interface.

| Preference | Type | Description |
|---|---|---|
| `insight.lists.ignorelist.paths` | stringlist | Path Ignore List (for example, /robots.txt,robots2.txt). Default: empty list. |
| `insight.lists.ignorelist.srcips` | stringlist | IP Ignore List (for example, 1.1.1.1,2.2.2.2). Default: empty list. |
| `insight.options.logformat` | string | Log Format. Accepts: auto, common, apache, iis, ncsa, nginx. Default: **auto**., which indicates it must auto detect the log format. |
| `insight.options.profileatk` | boolean | **true** (default): Build attacker profile.<br>**false**: Disables attacker profiling. |
| `insight.options.resolveip` | boolean | **true**: Resolve attacker IP addresses (slower).<br>**false** (default): Disables IP resolving (faster). |

---

<a id="prefs_hybrid"></a>

## Other Hybrid preferences

These global preferences apply to every scan, however it is started (the `scancode` or `scanurl` commands, the Web UI, the REST API, the Lua API, the PowerShell module or the classic interface).

| Preference | Type | Description |
|---|---|---|
| `hybrid.report.defaultlanguage` | string | Report Language ID. See the list of [available report languages](../integrations/command-line.md#report). Default: **EN**., for English language reports. |
| `hybrid.report.company.logo.urldark` | string | Organization Logo URL for dark background (for example, `http://www.mycompany.com/logo.png`). Default: empty. |
| `hybrid.lists.riskfine` | stringlist | Risk Redefinition. Each line must use the format [unique check ID=severity] (for example, C-1603660271-7241=low). Default: empty list. |
| `syhunt.hybrid.advanced.ai.openai.apikey.encrypted` | string | OpenAI Key for enabling Syhunt's AI-powered capabilities. Default: empty. |
| `syhunt.hybrid.advanced.ai.options.exrepsolut` | boolean | **true** (default): Share excerpts of vulnerable code to get patch examples. Requires `ai_reviewconsent: true` in a `.syhunt-ci.yml` file at the root of each target repository.<br>**false**: Disables share excerpts of vulnerable code to get patch examples, even if patch consent files are available. |

---

<a id="prefs_tracker"></a>

## Tracker preferences

Tracker preferences are set with `scancore -tracker:set`, using `-to` to name the tracker and `-v` for the value ([examples](../integrations/command-line.md#cli_trackers)). They apply only to that tracker.

For examples of creating trackers and setting their preferences, see [Command Line: using issue trackers](../integrations/command-line.md#clitrackerexbegin).


Which keys apply depends on the tracker type: for example, an email tracker uses `smtp.targetport`, while a GitHub tracker ignores it. The sections after the table list the keys each tracker type usually needs.

| Preference | Type | Description |
|---|---|---|
| `project.name` | string | Project Name (for example, owner/repo). Default: empty. |
| `auth.username` | string | Username. Default: empty. |
| `auth.password` | string | Password. Default: empty. |
| `auth.token.encrypted` | string | Authentication Token. This can be used to set a PAT (Personal Access Token) for a service. Default: empty. |
| `auth.targethost` | string | Hostname. Default: empty. |
| `api.url` | string | API/Server URL. Default: empty. |
| `custom.fields` | stringlist | Custom Fields. Default: empty list. |
| `custom.labels` | stringlist | Custom Labels. Default: empty list. |
| `scan.minseverity` | string | Minimal Severity. Accepted values are: Info, Low, Medium or High. Default: **Info**. |
| `notifyonfailonly` | boolean | **true**: Notify only when a fail condition is met.<br>**false** (default): Always notify (not tied to any pre-configured pass/fail conditions). |

### Keys by tracker type

| Tracker | Keys to set |
|---|---|
| **Email** | `auth.targethost` (SMTP host), `auth.username` and `auth.password` (credentials), plus the email keys below. |
| **GitHub** | `project.name` (`owner/repo`), `auth.token.encrypted` (personal access token), optionally `custom.labels` (for example, `Vulnerability`). |
| **GitLab** | `project.name` (`owner/repo`), `api.url` (GitLab server URL), `auth.token.encrypted` (personal access token), optionally `custom.labels`. |
| **Jira** | `project.name` (Jira project), `api.url` (Jira server URL), `auth.username` and `auth.password`, optionally `custom.labels`, `custom.fields` and `jira.defitname`. |
| **DefectDojo** | `api.url` (server URL), `auth.token.encrypted` (API token), optionally `scan.minseverity`, `custom.labels` (tags, for example `scan`) and the DefectDojo keys below. |
| **Faraday** | `api.url` (server URL), `project.name` (workspace; defaults to `my_workspace`), `auth.username` and `auth.password`. |

### Email

| Preference | Type | Description |
|---|---|---|
| `smtp.targetport` | integer | SMTP port. Default: **587**. |
| `message.from` | string | Sender address. Default: empty. |
| `message.tolist` | string | Recipient address, or a comma-separated list of addresses. Default: empty. |
| `message.subject` | string | Message subject. Default: empty. |

### Jira

| Preference | Type | Description |
|---|---|---|
| `jira.defitname` | string | Default issue type. Default: **Task**. |

Use `custom.fields` to reference custom fields by name rather than by ID. For a SelectList custom field, declare it as `customfield_10100.value=ACustomValue`.

### DefectDojo

| Preference | Type | Description |
|---|---|---|
| `scan.testtitle` | string | Context: test title. Default: empty. |
| `scan.engagementname` | string | Context: engagement name. Default: empty. |
| `scan.producttypename` | string | Context: product type name. Default: empty. |
| `scan.productname` | string | Context: product name. Default: empty. |
| `scan.active` | boolean | **true** (default): mark findings as active.<br>**false**: mark them as inactive. |
| `scan.verified` | boolean | **true** (default): mark findings as verified.<br>**false**: mark them as unverified. |
| `scan.skipduplicates` | boolean | **true**: skip duplicates.<br>**false** (default): don't skip duplicates. |
| `scan.closeoldfindings` | boolean | **true**: close old findings.<br>**false** (default): don't close old findings. |
| `scan.pushtojira` | boolean | **true**: push findings to Jira.<br>**false** (default): don't push to Jira. |
