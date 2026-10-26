# Web API

> **Applies to:** Syhunt Hybrid 6.9.8.2

**Contents**

- [Introduction](#wapi_intro_begin)
- [Launching a dynamic scan](#wapi_dynamic_begin)
- [Launching a source code scan](#wapi_code_begin)
- [Getting results](#wapi_getresults_begin)

<a id="wapi_intro_begin"></a>

## Introduction

Syhunt Hybrid Platinum includes a simple web API for launching dynamic and source code scans and retrieving the status, report and log of a scan session. Requests are `POST` requests with a JSON body, and responses are JSON, except when you request a report, which is returned as XML or JSON.

The same operations are available from Lua; see the [Lua API](lua-api.md).

### Before you start

Make sure the Syhunt API server is running and that you have an API key.

- **Linux:** start the API server with `scancore -apisignal:start` and generate an API key with `scancore -apikeygen`.
- **Windows:** in the Syhunt installation folder (usually `C:\Program Files\Syhunt Hybrid\`), open the `Server` subfolder and double-click the `nginx` executable to start the server. To get an API key, launch Syhunt Hybrid and choose **Help → View Web API Key**.

  ![](../images/hybrid_viewwebapikey.png)

Then open `http://localhost:8017/` and check that you see the NGINX default page. The API is served at:

- `http://<hostname>:8017/syhunt/launch.lua`
- `http://<hostname>:8017/syhunt/results.lua`

Every request to these endpoints must include a valid API key in its body (`apikey`).

<a id="wapi_dynamic_begin"></a>

## Launching a dynamic scan

**Endpoint:** `POST /syhunt/launch.lua`

```json
{
  "sessionname": "Test",
  "starturl": "http://127.0.0.1",
  "huntmethod": "appscan",
  "reporttemplate": "Complete",
  "apikey": "YOUR_API_KEY"
}
```

| Key | Description |
|---|---|
| `starturl` **(required)** | The target URL. |
| `apikey` **(required)** | Your Syhunt Web API key (see [Before you start](#wapi_intro_begin)). |
| `sessionname` | A unique, alphanumeric session name. If omitted, a random one is generated. |
| `huntmethod` | A valid [hunt method](../scanning/hunt-methods.md) name. |
| `reporttemplate` | A report template: `Standard`, `Comparison`, `Compliance` or `Complete`. |
| `reportgl` | If `true`, returns JSON output in GitLab format. |
| `timelimit` | Maximum scan time, for example `1d`, `3h`, `2h30m`, `50m`. The scan is aborted when it's reached. No limit by default. |

**Response:**

```json
{
  "huntmethod": "appscan",
  "pid": 5108,
  "result": true,
  "resultstr": "",
  "sessionname": "Test",
  "sessiontype": "dynamic"
}
```

<a id="launchresponse"></a>

| Key | Description |
|---|---|
| `pid` | A unique process ID for the running scan. |
| `result` | `true` if the scan was launched; `false` if it couldn't be started. |
| `resultstr` | If `result` is `false`, a description of the error. |
| `sessionname` | The scan's session name. If you didn't provide one, this is the generated name. Use it in later requests to get the results. |
| `sessiontype` | The session type: `dynamic` or `code`. |

<a id="wapi_code_begin"></a>

## Launching a source code scan

**Endpoint:** `POST /syhunt/launch.lua`

Local folder:

```json
{
  "sessionname": "Test",
  "sourcetarget": "P:\\Private\\MyWebApp\\",
  "huntmethod": "normal",
  "reporttemplate": "Complete",
  "apikey": "YOUR_API_KEY"
}
```

Repository URL:

```json
{
  "sessionname": "Test",
  "sourcetarget": "https://github.com/drnic/php-helloworld.git",
  "sourcebranch": "master",
  "huntmethod": "normal",
  "reporttemplate": "Complete",
  "apikey": "YOUR_API_KEY"
}
```

| Key | Description |
|---|---|
| `sourcetarget` **(required)** | A local folder or a Git URL. |
| `apikey` **(required)** | Your Syhunt Web API key. |
| `sourcebranch` | The branch to scan, for repository URLs. |
| `sessionname`, `huntmethod`, `reporttemplate`, `reportgl`, `timelimit` | As for [dynamic scans](#wapi_dynamic_begin). |

**Response:**

```json
{
  "huntmethod": "normal",
  "pid": 4476,
  "result": true,
  "resultstr": "",
  "sessionname": "Test2",
  "sessiontype": "code"
}
```

The response keys are the same as for [dynamic scans](#launchresponse).

<a id="wapi_getresults_begin"></a>

## Getting results

All result requests go to **`POST /syhunt/results.lua`** and take:

| Key | Description |
|---|---|
| `sessionname` **(required)** | The session whose results you want. |
| `resulttype` **(required)** | `status`, `report_xml`, `report_json` or `session_log`. |
| `apikey` **(required)** | Your Syhunt Web API key. |

### Status

```json
{
  "sessionname": "Test",
  "resulttype": "status",
  "apikey": "YOUR_API_KEY"
}
```

**Response:**

```json
{
  "report_xml_available": true,
  "report_json_available": true,
  "session_log_available": true,
  "sessionname": "Test",
  "status": "Completed"
}
```

| Key | Description |
|---|---|
| `report_xml_available` | `true` if an XML report is available. |
| `report_json_available` | `true` if a JSON report is available. |
| `session_log_available` | `true` if a scan log is available. |
| `status` | The session status: `Scanning`, or `Completed` once the scan has finished. |

### Report

Request `report_xml` or `report_json`:

```json
{
  "sessionname": "Test",
  "resulttype": "report_xml",
  "apikey": "YOUR_API_KEY"
}
```

With `report_xml`, the response body is the scan report in XML:

```xml
<?xml version="1.0"?>
<report>
  <report_title>Syhunt Scanner Report</report_title>
  <!-- ... -->
  <scanner_version>6.5.0.0</scanner_version>
</report>
```

### Log

```json
{
  "sessionname": "Test",
  "resulttype": "session_log",
  "apikey": "YOUR_API_KEY"
}
```

The response body is the scan log, in plain text.
