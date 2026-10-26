# Lua API

> **Applies to:** Syhunt Hybrid 6.9.12

**Contents**

- [Introduction](#lapi_intro_begin)
- [Launching a dynamic scan](#lapi_dynamic_begin)
- [Launching a source code scan](#lapi_code_begin)
- [Getting results](#lapi_getresults_begin)
- [Generating a report or export](#lapi_genreport_begin)
- [Getting session details](#lapi_getsessiondetails_begin)
- [Emailing a report](#lapi_emailreport_begin)
- [Updating preferences](#lapi_prefupdate)
- [Other functions](#lapi_extras)

<a id="lapi_intro_begin"></a>

## Introduction

Syhunt Hybrid Platinum includes a simple Lua API for launching dynamic and source code scans, retrieving the status, report and log of a scan session, and more. The same operations are available over HTTP through the [Web API](web-api.md).

### Loading the Syhunt library

Load the Syhunt library before using the API:

```lua
-- From a GUI app
require "SyMini"
-- From a console app
require "SyMini.Console"
```

<a id="lapi_dynamic_begin"></a>

## Launching a dynamic scan

```lua
local res = symini.scantask_launch({
  sessionname = "Test",
  starturl = "http://127.0.0.1",
  huntmethod = "appscan",
  reporttemplate = "Complete"
})
if res.result == true then
  print(res.sessionname)
end
```

<a id="lapi_dynlaunch_begin"></a>

| Key | Description |
|---|---|
| `starturl` **(required)** | The target URL. |
| `sessionname` | A unique, alphanumeric session name. If omitted, a random one is generated. |
| `huntmethod` | A valid [hunt method](../scanning/hunt-methods.md) name. |
| `reporttemplate` | A report template: `Standard`, `Comparison`, `Compliance` or `Complete`. |
| `reportgl` | If `true`, returns JSON output in GitLab format. |
| `timelimit` | Maximum scan time, for example `1d`, `3h`, `2h30m`, `50m`. The scan is aborted when it's reached. No limit by default. |

<a id="lapi_dynlaunch_end"></a>
<a id="lapi_dynlaunchret_begin"></a>

**Returned table:**

| Key | Description |
|---|---|
| `pid` | A unique process ID for the running scan. |
| `result` | `true` if the scan was launched; `false` if it couldn't be started. |
| `resultstr` | If `result` is `false`, a description of the error. |
| `sessionname` | The scan's session name. If you didn't provide one, this is the generated name. Use it in later calls to get the results. |
| `sessiontype` | The session type: `dynamic` or `code`. |

<a id="lapi_dynlaunchret_end"></a>
<a id="lapi_code_begin"></a>

## Launching a source code scan

```lua
-- Scan a local folder
local res = symini.scantask_launch({
  sessionname = "Test",
  sourcetarget = "P:\\Private\\MyWebApp\\",
  huntmethod = "normal",
  reporttemplate = "Complete"
})

-- Scan a repository URL
local res = symini.scantask_launch({
  sessionname = "Test",
  sourcetarget = "https://github.com/drnic/php-helloworld.git",
  sourcebranch = "master",
  huntmethod = "normal",
  reporttemplate = "Complete"
})

if res.result == true then
  print(res.sessionname)
end
```

<a id="lapi_codelaunch_begin"></a>

| Key | Description |
|---|---|
| `sourcetarget` **(required)** | A local folder or a Git URL. |
| `sourcebranch` | The branch to scan, for repository URLs. |
| `sessionname`, `huntmethod`, `reporttemplate`, `reportgl`, `timelimit` | As for [dynamic scans](#lapi_dynlaunch_begin). |

<a id="lapi_codelaunch_end"></a>
<a id="lapi_codelaunchret_begin"></a>

The returned table is the same as for [dynamic scans](#lapi_dynlaunchret_begin).

<a id="lapi_codelaunchret_end"></a>
<a id="lapi_getresults_begin"></a>

## Getting results

### Status

```lua
local res = symini.scantask_getresults({
  sessionname = "Test",
  resulttype = "status"
})
```

<a id="lapi_results_begin"></a>

| Key | Description |
|---|---|
| `sessionname` **(required)** | The session whose results you want. |
| `resulttype` **(required)** | `status`, `report_xml`, `report_json` or `session_log`. |

<a id="lapi_results_end"></a>
<a id="lapi_resultsret_begin"></a>

**Returned table:**

| Key | Description |
|---|---|
| `report_xml_available` | `true` if an XML report is available. |
| `report_json_available` | `true` if a JSON report is available. |
| `session_log_available` | `true` if a scan log is available. |
| `status` | The session status: `Scanning`, or `Completed` once the scan has finished. |

<a id="lapi_resultsret_end"></a>

### Report

```lua
-- XML report
local res = symini.scantask_getresults({
  sessionname = "Test",
  resulttype = "report_xml"
})

-- JSON report
local res = symini.scantask_getresults({
  sessionname = "Test",
  resulttype = "report_json"
})
```

### Log

```lua
local res = symini.scantask_getresults({
  sessionname = "Test",
  resulttype = "session_log"
})
```

Returns the scan log as text.

<a id="lapi_genreport_begin"></a>

## Generating a report or export

```lua
local res = symini.genreport({
  sessionname = 'mysessionname',
  outfilename = 'myreport.pdf',
  template = 'Standard'
})
```

| Key | Description |
|---|---|
| `sessionname` **(required)** | The session to report on. |
| `outfilename` **(required)** | The report's output file name; the extension sets the [format](command-line.md#repfmt). |
| `template` | The [report template](command-line.md#reptemplates). Default: `Standard`. |
| `passfailcond` | A [pass/fail condition](command-line.md#pfcondbegin). |

**Returned table:**

| Key | Description |
|---|---|
| `outfilename` | The report's full output path. |
| `result` | `true` if the report was generated. |
| `resultstr` | Status of the report generation. |
| `passfail_result` | `true` if the pass/fail condition passed. |
| `passfail_status` | Status of the pass/fail condition. |
| `passfail_resultstr` | Reason for the pass/fail result. |

<a id="lapi_getsessiondetails_begin"></a>

## Getting session details

```lua
local res = symini.getsessiondetails('mysessionname')
```

**Returned table:**

| Key | Description |
|---|---|
| `huntmethod` | Hunt method. |
| `datetime` | Date and time of the scan. |
| `duration` | Duration of the scan. |
| `targets` | Target(s) of the scan. |
| `targetdesc` | Description of the target. |
| `sourcedir` | Target source code folder, if any. |
| `targetfile` | Target file, if any. |
| `targeturl` | Target URL, if any. |
| `vulncount` | Number of vulnerabilities. |
| `attackcount` | Number of attacks (web server log scans). |
| `ports` | Target ports, if any. |
| `status` | Session status. |
| `resultsdesc` | Description of the results. |
| `launcher` | Name of the tool that launched the session. |

<a id="lapi_emailreport_begin"></a>

## Emailing a report

```lua
local res = symini.emailreport({
  tracker = 'myemailtrackername',
  filename = 'myreport.pdf',
  subject = 'My Test Report'
})
```

| Key | Description |
|---|---|
| `tracker` **(required)** | The name of an [email tracker](command-line.md#cli_trackers) you've set up. |
| `filename` **(required)** | Full path of the report to send. |
| `subject` | Email subject. Default: `Syhunt Scanner Report`. |

**Returned table:**

| Key | Description |
|---|---|
| `result` | `true` if the email was sent. |
| `resultstr` | Status, including any error message. |
| `resultdbg` | Debug log of the operation. |

<a id="lapi_prefupdate"></a>

## Updating preferences

| Function | Description |
|---|---|
| `symini.prefs_set(key, value [, targeturl])` | Sets a persistent preference, globally or for a target. |
| `symini.prefs_get(key [, targeturl])` | Gets a persistent preference, globally or for a target. |

See [Preferences](../reference/preferences.md) for the available preference IDs.

```lua
-- Update a site preference
local res = symini.prefs_set('enabled', true, 'http://127.0.0.1')
-- Update a global preference
local res = symini.prefs_set('hybrid.report.company.logo.url', 'https://www.mydomain.com/mylogo.png')
if res.result == true then
  print('Preference updated.')
end
```

**Returned table:**

| Key | Description |
|---|---|
| `result` | `true` if the operation succeeded. |
| `resultstr` | Status, including any error message. |
| `value` | The preference's value (`prefs_get` only). |

<a id="lapi_extras"></a>

## Other functions

| Function | Description |
|---|---|
| `symini.info.version` | The Syhunt version. |
| `symini.getsessionname()` | Returns a random session name you can use to launch a scan. |
| `symini.getsessioncomparison(sessionname1, sessionname2)` | Returns a comparison of two sessions. |
| `symini.getsessionlist([maxdays])` | Returns the list of past sessions. |
| `symini.runcmd(cmdname)` | Runs a command (see below). |

Commands available to `symini.runcmd`:

| Command | Effect |
|---|---|
| `stop` | Stops all running processes of this Syhunt installation. |
| `clearinc` | Clears the incremental scan cache. |
| `clearpref` | Clears the global preferences. |
| `clearsite` | Clears the site preferences. |
| `cleartrack` | Removes all issue trackers. |

More examples, including object-based usage, are in the [scantools repository](https://github.com/syhunt/scantools/tree/master/src).
