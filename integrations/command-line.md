# Command Line (CLI)

> **Applies to:** Syhunt Hybrid 7.2

**Contents**

- [Introduction](#intro)
- [Dynamic scans](#cli_dynamic_begin)
- [API scans](#cli_api_begin)
- [Source code scans](#cli_code_begin)
- [Forensic scans](#cli_forensic)
- [Pass/fail conditions](#pfcond)
- [Incremental scan modes](#incremental)
- [Report formats, templates and languages](#report)
- [Modifying Syhunt preferences](#cli_prefs)
- [Using issue trackers](#cli_trackers)
- [Updating Syhunt](#cli_update)

<a id="intro"></a>

## Introduction

This guide explains how to run scans and generate vulnerability reports from the command line.

The command-line tools are in the Syhunt installation folder, which depends on your operating system and edition:

| OS | Default location |
|---|---|
| Windows | `C:\Program Files\Syhunt Hybrid\`<br>`C:\Program Files (x86)\Syhunt Hybrid\`<br>`C:\Program Files\Syhunt Community` |
| Linux | `/home/<user>/syhunt-hybrid/carbon/`<br>`/home/<user>/syhunt-community/carbon/` |
| macOS | `/Applications/Syhunt Hybrid/carbon`<br>`/Applications/Syhunt Community/carbon` |

| Tool | Purpose |
|---|---|
| `scanurl` | Dynamic (DAST) and API scans |
| `scancode` | Source code scans (SAST/MAST) |
| `scanlog` | Forensic log analysis |
| `scancore` | Preferences, trackers and maintenance commands |
| `scanupdate` | Updates (Linux and macOS) |

Open a terminal or Command Prompt in the installation folder before running the commands below.

<a id="cli_dynamic_begin"></a>

## Dynamic scans

```
scanurl <starturl> -hm:<huntmethod>
```

Example:

```
scanurl http://www.somehost.com -hm:appscan
```

`scanurl` automatically generates and saves a report unless you pass `-nr`.

<a id="cli_dynamic_parameters"></a>

### `scanurl` parameters

All parameters are optional.

| Parameter | Description | Default |
|---|---|---|
| `-sn:<name>` | Session name; must be unique. If omitted, a unique ID is generated. | Generated ID |
| `-hm:<name>` | [Hunt method](../scanning/hunt-methods.md) to use. | `appscan` |
| `-emu:<mode>` | Browser emulation mode: `chrome`, `edge`, `firefox`, `msie` or `safari`. | `chrome` |
| `-srcdir:<dir>` | Target code folder for a hybrid scan (for example `"C:\www\docs\"` or `/home/user/www/`). | |
| `-tk:<tracker>` | Sends vulnerabilities to a tracker after the scan. Can be combined with `-pfcond`. | |
| `-tk2:<tracker>`, `-tk3:<tracker>` | Additional trackers, as above. | |
| `-nr` | Doesn't generate a report after the scan. | |
| `-or` | Opens the report after generating it. | |
| `-rout:<filename>` | Report file name; the extension sets the [format](#repfmt). | `Report_<session>.html` |
| `-rtpl:<name>` | [Report template](#reptemplates). | `Standard` |
| `-xout:<filename>` | Export file name; the extension sets the [format](#repfmt). | `Export_<session>.xml` |
| `-xout2:<filename>` | A second export file. | `Export_<session>.xml` |
| `-pfcond:<condition>` | [Pass/fail condition](#pfcond) to report. | |
| `-nv` | Less verbose output (errors and basic information are still printed). | |
| `-inc:<mode>` | [Incremental scan mode](#incremental). | `targetpref` |
| `-inctag:<name>` | Stores the incremental scan data under a tag. | |
| `-mnl:<n>` | Maximum number of links per server. | `10000` |
| `-mnr:<n>` | Maximum number of retries. | `2` |
| `-tmo:<ms>` | Request timeout, in milliseconds. | `8000` |
| `-tml:<time>` | Maximum scan time, for example `1d`, `3h`, `2h30m`, `50m`. | No limit |
| `-ver:<v>` | HTTP version. | `1.1` |
| `-nofris` | Doesn't automatically follow an off-domain redirect at the start URL. | |
| `-nodos` | Disables denial-of-service tests. | |
| `-nojs` | Disables JavaScript emulation and execution. | |
| `-atype:<type>` | Authentication type: `Basic`, `Form` or `Manual`. | |
| `-auser:<username>` | Username for authentication. | |
| `-apass:<password>` | Password for authentication. | |
| `-about` | Shows version information. | |
| `-help` (or `/?`) | Lists the available parameters. | |

### Scanning IPv6 addresses

Enclose IPv6 addresses in square brackets, for example `http://[2001:4860:0:2001::68]/index.php`.

<a id="cli_dynamic_end"></a>
<a id="cli_api_begin"></a>

## API scans

API scans use `scanurl` too. The only difference is that you pass the URL of the **API specification** instead of a start URL. If you pass the API's own URL, Syhunt runs a traditional dynamic scan instead, without API mapping, schema validation or API-specific tests.

For example, if the specification is at `https://example.com/openapi.json` and describes an API at `https://api.example.com`:

```
scanurl https://example.com/openapi.json -hm:appscan   # correct: API scan
scanurl https://api.example.com                        # wrong: runs a traditional DAST scan
```

`scanurl` accepts the same [optional parameters](#cli_dynamic_parameters) for API scans. See [Web API Scanning](../scanning/api-scanning.md) for details.

<a id="cli_api_end"></a>
<a id="cli_code_begin"></a>

## Source code scans

```
scancode <target> -hm:<huntmethod>
```

Examples:

```bat
:: Git repositories
scancode git://sub.domain.com/repo.git
scancode https://github.com/user/repo.git -rb:master

:: Local paths
scancode /source/www/
scancode c:\source\www\
scancode c:\source\www\file.php
scancode c:\mobile\myapp.apk
scancode "c:\source code\www\"
```

<a id="cli_code_windows"></a>

Azure DevOps / TFS repositories:

```bat
scancode https://dev.azure.com/user/project
scancode https://myserver/tfs/project
scancode collection:https://dev.azure.com/user$/project
```

`scancode` automatically generates and saves a report unless you pass `-nr`.

### `scancode` parameters

All parameters are optional.

| Parameter | Description | Default |
|---|---|---|
| `-sn:<name>` | Session name; must be unique. If omitted, a unique ID is generated. | Generated ID |
| `-hm:<name>` | [Hunt method](../scanning/hunt-methods.md) to use. | `appscan` |
| `-rb:<branch>` | Git repository branch. | |
| `-tfsv:<version>` | TFS version. | `default` |
| `-tk:<tracker>` | Sends vulnerabilities to a tracker after the scan. Can be combined with `-pfcond`. | |
| `-tk2:<tracker>`, `-tk3:<tracker>` | Additional trackers, as above. | |
| `-nr` | Doesn't generate a report after the scan. | |
| `-or` | Opens the report after generating it. | |
| `-rout:<filename>` | Report file name; the extension sets the [format](#repfmt). | `Report_<session>.html` |
| `-rtpl:<name>` | [Report template](#reptemplates). | `Standard` |
| `-xout:<filename>` | Export file name; the extension sets the [format](#repfmt). | `Export_<session>.xml` |
| `-xout2:<filename>` | A second export file. | `Export_<session>.xml` |
| `-pfcond:<condition>` | [Pass/fail condition](#pfcond) to report. | |
| `-nv` | Less verbose output (errors and basic information are still printed). | |
| `-inc:<mode>` | [Incremental scan mode](#incremental). | `targetpref` |
| `-inctag:<name>` | Stores the incremental scan data under a tag. | |
| `-excp:<paths>` | Paths to exclude from the analysis, for example `-excp:/path/*,/path2/*`. | |
| `-refurl:<url>` | A URL associated with the code, for reference only. | |
| `-noifa` | Disables input filtering analysis. | |
| `-tml:<time>` | Maximum scan time, for example `1d`, `3h`, `2h30m`, `50m`. | No limit |
| `-about` | Shows version information. | |
| `-help` (or `/?`) | Lists the available parameters. | |

<a id="cli_code_end"></a>
<a id="cli_forensic"></a>

## Forensic scans

Forensic log analysis is run with `scanlog`. In Syhunt Hybrid 7.2, this is the only way to run Syhunt Forensic. See [Forensic Log Analysis](../scanning/forensic-log-analysis.md). Run `scanlog -help` to list its parameters.
<a id="pfcond"></a>

## Pass/fail conditions

Pass a pass/fail condition to `scanurl` or `scancode` with the `-pfcond` parameter.

<a id="pfcondbegin"></a>

| Condition | Fails if… |
|---|---|
| `high` or `fail-if:risk=high` | a High-risk vulnerability or threat is found. |
| `medium` or `fail-if:risk=mediumup` | a Medium- or High-risk vulnerability or threat is found. |
| `low` or `fail-if:risk=lowup` | a Low-, Medium- or High-risk vulnerability or threat is found. |

<a id="pfcondend"></a>
<a id="incremental"></a>

## Incremental scan modes

| Mode | Description |
|---|---|
| `auto` | Manages the incremental scan cache automatically (recommended). |
| `disabled` | Disables the incremental scan cache. Scans take 3 to 4 times longer. |
| `forced` | Always uses incremental scanning. If you use this mode, also run a regular, non-forced scan about once a month. |
| `targetpref` | Uses the mode set in the target's preferences. |

To reset the incremental scan cache, run `scancore -runcmd:clearinc`.

<a id="report"></a>

## Report formats, templates and languages

<a id="replanguages"></a>

### Languages

| Code | Language |
|---|---|
| `EN` | English |
| `DE` | German |
| `ES` | Spanish |
| `FR` | French |
| `IT` | Italian |
| `JP` | Japanese |
| `KR` | Korean |
| `PT` | Portuguese |

To set the report language:

```
scancore -prefset:hybrid.report.defaultlanguage -v:EN
```

You can also set it in the interface: ![](../images/menu.png) **Menu → Preferences → Other Hybrid Preferences**.

<a id="repfmt"></a>

### Formats

The output format is set by the file extension passed to `-rout` or `-xout`:

| Extension | Format |
|---|---|
| `.html` | HTML report |
| `.pdf` | PDF report |
| `.xml` | XML export |
| `.json` | JSON vulnerability export |
| `.csv` | CSV (comma-separated values) |
| `.mse.csv` | CSV for Microsoft Excel |
| `.txt` | Text report |

<a id="reptemplates"></a>

### Templates

| Template | Contents |
|---|---|
| `Standard` | Low- to high-risk vulnerability information. |
| `Comparison` | Standard content plus how the vulnerabilities evolved between scans. |
| `Compliance` | OWASP Top 10, CWE/SANS Top 25 (2019) and PCI DSS 3.2.1 compliance information. |
| `Mobile` | OWASP Mobile Top 10, CWE/SANS Top 25 (2019) and PCI DSS 3.2.1 compliance information. |
| `Complete` | Standard content plus comparison, compliance, request/response and coverage details. |

<a id="huntmethods_begin"></a>

For the available hunt methods and their CLI names, see [Hunt Methods](../scanning/hunt-methods.md).

<a id="cli_prefs"></a>

## Modifying Syhunt preferences

Since Syhunt 6.9.6.3, you can change Syhunt's [persistent preferences](../reference/preferences.md), globally or for a specific target, with `scancore -prefset`. Use `-prefprint` to print the current value of any preference.

<a id="cli_prefs_exbegin"></a>

```lua
-- Example 1: set the logo shown in new reports
scancore -prefset:hybrid.report.company.logo.urldark -v:https://www.mydomain.com/mylogo.png

-- Example 2: print the current report logo URL
scancore -prefprint:hybrid.report.company.logo.urldark

-- Example 3: set Basic authentication for a specific site
scancore -tg:http://127.0.0.1 -prefset:dynamic.formauth.type -v:Basic
scancore -tg:http://127.0.0.1 -prefset:dynamic.formauth.username -v:myuser
scancore -tg:http://127.0.0.1 -prefset:dynamic.formauth.password -vsecret

-- Example 4: enable or disable the preferences of a specific site
scancore -tg:http://127.0.0.1 -prefset:enabled -v:true
scancore -tg:http://127.0.0.1 -prefset:enabled -v:false

-- Example 5: set a value from a list file
scancore -tg:http://127.0.0.1 -prefset:dynamic.lists.cookies -fromfile:mycookies.lst
```

<a id="cli_prefs_exend"></a>

See [Preferences](../reference/preferences.md) for the full list of preference IDs.

### Clearing all preferences

```lua
-- Clear global preferences
scancore -runcmd:clearpref
-- Clear the preferences of all sites
scancore -runcmd:clearsite
```

<a id="cli_trackers"></a>

## Using issue trackers

Since Syhunt 6.9.6.3, you can add, manage and use [issue trackers](issue-trackers.md) from the command line with `scancore -tracker`.

<a id="clitrackerexbegin"></a>

```lua
-- Example 1: add a GitHub tracker
scancore -tracker:add
-- Enter the type (GitHub) and a name for the tracker, then press Enter.
-- Configure it:
scancore -tracker:set to:mytrackername -key:project.name -v:owner/repo
scancore -tracker:set to:mytrackername -key:auth.token.encrypted -vsecret

-- Example 2: add an email tracker
scancore -tracker:add
-- Enter the type (Email) and a name for the tracker, then press Enter.
scancore -tracker:set to:myemailtracker -key:message.from -v:robot@yourdomain.com
scancore -tracker:set to:myemailtracker -key:message.tolist -v:security@yourdomain.com,team@yourdomain.com
scancore -tracker:set to:myemailtracker -key:auth.targethost -v:smtp.yourdomain.com
scancore -tracker:set to:myemailtracker -key:smtp.targetport -v:587
scancore -tracker:set to:myemailtracker -key:auth.username -v:myusername
scancore -tracker:set to:myemailtracker -key:auth.password -vsecret

-- Example 3: test a tracker
scancore -tracker:send -tid:TEST -to:mytrackername -note:"My comment"

-- Example 4: send a vulnerability from a report to a tracker
-- 1596281007-7-4771 is the vulnerability's track ID, taken from the report.
scancore -tracker:send -tid:1596281007-7-4771 -to:mytrackername -note:"My comment"

-- Example 5: list trackers
scancore -tracker:list

-- Example 6: delete a tracker
scancore -tracker:del to:mytrackername
```

<a id="clitrackerexend"></a>

See [Preferences: trackers](../reference/preferences.md#prefs_tracker) for the list of tracker preference IDs.

<a id="cli_trackers_send"></a>

### Sending vulnerabilities to a tracker after a scan

Pass `-tk` to `scanurl` or `scancode` to send a summary of the vulnerabilities found to a tracker:

```lua
-- Example 1: use an existing tracker
scanurl mydomain -tk:trackername

-- Example 2: send only if high-severity vulnerabilities are found
scanurl mydomain -tk:trackername -pfcond:fail-if:risk=high

-- Example 3: define the tracker on the fly
scanurl mydomain -tk:"?app=github###project=user/repo###token=ghp_etc"
```

> [!NOTE]
> When you pass tracker preferences in `-tk`, use the [short versions of their names](issue-trackers.md#shortnames).

### Removing all trackers

```
scancore -runcmd:cleartrack
```

<a id="cli_update"></a>

## Updating Syhunt

On Linux and macOS, run `scanupdate` to check for updates; if any are available, Syhunt asks whether to download and install them. Run `scanupdate auto` to install them without confirmation.

On Windows, download and install updates from the Syhunt website.
