# Web API Scanning

> **Applies to:** Syhunt API 7.2

**Contents**

- [How API scanning works](#api_begin)
- [Supported specification formats](#specformats)
- [Running an API scan](#running)
- [Demo specifications](#demospecs)
- [Preventing a vulnerability from being reported](#advanced_vulnignore)
- [Hunt methods for APIs](#huntmethods_begin)
- [Scheduling API scans](#scheduling_begin)
- [Pre-scan requirements](#apiscanreq)
- [Hosting and uploading specifications](#specsources)
- [Safeguards against accidental scanning](#safeguards)
- [Specification format requirements](#specreqs)

<a id="begin"></a>
<a id="api_begin"></a>

## How API scanning works

An API scan works much like a [traditional dynamic scan](dynamic-scanning.md): Syhunt injects test data into the API's endpoints and analyzes the responses to find out whether the API is vulnerable to specific application-level attacks.

The difference is the starting point. A dynamic scan starts from a URL and crawls the site; an API scan starts from an **API specification** (a URL or a file). Instead of crawling, Syhunt maps the API from the specification and then runs a **schema validation** step, a form of functional testing that confirms the API exists and behaves as specified before the security tests begin. If it finds a major mismatch, the scan stops automatically and shows a warning, so you don't get misleading results.

<a id="specformats"></a>

## Supported specification formats

- API Blueprint
- OpenAPI 2 and 3
- Swagger 1, 2 and 3
- Postman Collection 1, 2 and 2.1
- GraphQL (with REST annotations)
- I/O Docs
- Google Discovery
- RAML
- WADL

<a id="specformatsend"></a>

Before scanning, Syhunt checks that the specification URL is valid and in a supported format, and rejects anything incompatible or invalid. The basic license supports the main formats (OpenAPI 2 and 3, Swagger 1 to 3, and API Blueprint), which together cover more than 95% of the API specifications in use today. Other licenses may add GraphQL, I/O Docs, Google Discovery, RAML and WADL.

Because APIs are black boxes, Syhunt needs a specification to scan one: it provides the structure and endpoint definitions that guide testing. Syhunt is broadly compatible, but we recommend [API Blueprint](https://apiblueprint.org/). Its clean, human-readable Markdown syntax, strong documentation features and easy integration into development and testing workflows fit Syhunt's focus on ease of use.

<a id="running"></a>

## Running an API scan

1. ⚠️ Make sure you meet the [pre-scan requirements](#apiscanreq) and are authorized to scan the target API. Syhunt includes [safeguards](#safeguards) against scanning APIs outside your direct control.
2. [Enable the Web UI](../installation/web-ui.md#firststeps), if you haven't done so yet.
3. In the Web UI, click API's **New Scan** button.<br>![](../images/newscanbutton_api.png)
4. Enter the URL of the API's specification, for example `http://mysite/api/openapi.json`. To try it out, use one of the [demo specifications](#demospecs).

   > [!NOTE]
   > You'll get an **Invalid target** error if you enter the API's own URL instead of its specification URL, or if the specification is invalid.
5. Select a scan method. We recommend **Application Scan (Default)**, which checks for all vulnerabilities using the recommended settings. See [Hunt Methods](hunt-methods.md).
6. *(Optional)* Click **Preferences** next to the target URL to customize the API's settings.
7. Click **Start Scan**.

You can click **View Report** at any time during the scan to see the results so far as an HTML report.

To scan from the command line, see [Command Line: API scans](../integrations/command-line.md#cli_api_begin).

<a id="demospecs"></a>

## Demo specifications

The Web UI includes simulated vulnerable APIs for demonstration and testing. Point the scanner to one of the specification URLs below. These specs reference locally accessible endpoints only and contain no real vulnerabilities; they just simulate vulnerable behavior so you can test and learn safely.

| Format | Specification URL |
|---|---|
| API Blueprint | `http://spec.syhunt.test/vulnapisim.apib` |
| OpenAPI 3 | `http://spec.syhunt.test/vulnapisim_openapi3.json` |
| Swagger 2 | `http://spec.syhunt.test/vulnapisim_swagger2.json` |
| Postman 2.1 | `http://spec.syhunt.test/vulnapisim_postman2.1.json` |
| Postman 1 | `http://spec.syhunt.test/vulnapisim_postman1.json` |

<a id="advanced_vulnignore"></a>

## Preventing a vulnerability from being reported

Findings from API scans can be suppressed [the same way as in dynamic scans](dynamic-scanning.md#advanced_vulnignore), using the ignore IDs shown in the reports.

<a id="huntmethods_begin"></a>

## Hunt methods for APIs

Most [hunt methods](hunt-methods.md) available for dynamic scans also work for API scans, with one important difference in the **Top 10** method:

- For web applications, it covers the **OWASP Top 10 for Web Applications**: injection, broken authentication, cross-site scripting and so on.
- For APIs, it covers **both** the OWASP Top 10 for Web Applications **and** the **OWASP API Security Top 10**, which adds API-specific risks such as broken object-level authorization (BOLA), excessive data exposure, lack of rate limiting and API-specific misconfigurations.

This reflects a principle at Syhunt: the classic OWASP Top 10 is still highly relevant to APIs, because many web application risks also show up at the API level. Applying both lists gives a truly comprehensive Top 10 assessment, covering the risks APIs inherit from web applications as well as those unique to APIs.

Methods that don't apply to APIs are unavailable. If you start an API scan with one of them, Syhunt aborts and shows a warning.

<a id="scheduling_begin"></a>

## Scheduling API scans

API scans are scheduled the same way as dynamic scans. See [Scheduling Scans](scheduling.md).

<a id="apiscanreq"></a>

## Pre-scan requirements

1. ⚠️ **An API specification (URL or file) is required.** Because APIs are black boxes, an accurate and complete scan is impossible without one: the specification defines the endpoints, parameters and expected behavior that guide testing.
   - Security teams don't always have access to API specifications. If that's your case, ask the development team for them before you start. Treat any API without proper documentation or a specification as a security risk, since it can't be effectively tested, reviewed or monitored.
2. ⚠️ **Only system administrators, or other people in charge of the target, should use this software. Do not scan APIs outside your direct control.**
   - If you need to scan an API you don't control, get written permission from its owner or administrator first.
   - Syhunt includes [safeguards](#safeguards) to prevent accidental scanning of APIs outside your organization's control.
   - As good practice, back up the API's source code and database before scanning. In rare cases, injection testing against an insecure, fragile API can pollute the database or alter files in ways that affect how the API works.
3. Make sure you meet the [Internet connection requirements](../installation/install-windows.md#sysreq_net).
4. Read and accept the [Syhunt EULA](https://www.syhunt.com/en/?n=Legal.EULA) before launching any scan.

<a id="specsources"></a>

## Hosting and uploading specifications

Since Syhunt 7.1.11, you can either link to a specification hosted anywhere or upload a specification file.

| Source | How it works | Includes (referenced files) |
|---|---|---|
| **Hosted** | Enter the specification's URL. | Supported, as long as every included file is in the same path as the main specification, or below it. |
| **Uploaded** | Syhunt serves the file at an internal URL such as `http://spec.syhunt.test/<specfilename.ext>`. | Not supported. The file must be self-contained. |
| **Local** (Windows only) | Place the file in `C:\ProgramData\Syhunt\UserData\Specs`. It becomes available at `http://localspec.syhunt.test/<specfilename.ext>`, accessible only from the internal network to reduce exposure. | Supported. Syhunt fetches the included files automatically. |

<a id="safeguards"></a>

## Safeguards against accidental scanning

Syhunt automatically aborts an API scan unless one of these conditions is met:

1. The API host referenced in the specification is on an internal network (a private IP range or a loopback address).
2. The API's domain has been explicitly allowed by adding it to the Targets list. For example, if the specification points to `api.mydomain.com`, add `mydomain.com` to the Targets list, either:
   - in the Web UI at `http://127.0.0.1:8017/syhunt/?n=targets`, or
   - from the CLI: `scancore -tg:https://mydomain.com -prefset:enabled -v:true`

If neither condition is met, an error message explains the problem.

This safeguard prevents users from unintentionally scanning external or third-party APIs that their organization doesn't control. It keeps scans within the authorized scope, protecting both the user and outside systems from unintended or unauthorized testing.

<a id="specreqs"></a>

## Specification format requirements

Syhunt makes a considerable effort to accept non-standard or loosely written specifications, but specifications that stray too far from their official format may be rejected, and Syhunt does not commit to supporting malformed or non-compliant specifications. If yours is rejected, correct it or use a third-party tool to make it compliant, or convert it to API Blueprint for the best compatibility and stability.

<a id="sysreq_begin"></a>

## System requirements

See [Technical Specifications](../reference/technical-specifications.md#specsbegin).
