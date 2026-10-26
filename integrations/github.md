# GitHub

> **Applies to:** Syhunt Hybrid 7.2

**Contents**

- [Introduction](#intro)
- [Activating a Syhunt runner](#activation)
- [Adding Syhunt to a workflow](#addyml)
- [Scan functions and pass/fail conditions](#dast)
- [Sending vulnerabilities to GitHub Issues](#submitissues)

<a id="intro"></a>

## Introduction

You can launch Syhunt scans from a GitHub Actions workflow, bringing Syhunt Dynamic, Syhunt Code and Syhunt Mobile into your continuous delivery pipeline. You can also set up GitHub as an issue tracker in Syhunt, so vulnerabilities can be submitted to a project's Issues.

The following workflow scans the repository's source code, fails the job if medium- or high-risk vulnerabilities are found, and attaches a PDF vulnerability report to the job's artifacts:

```yaml
name: syhunt test
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: [self-hosted, Windows, X64]
    steps:
      - uses: actions/checkout@v2
      - run: Start-CodeScan -pfcond 'medium' -output 'report.pdf'
      - name: 'Syhunt Report'
        uses: actions/upload-artifact@v2
        if: failure()
        with:
          name: syhunt-report
          path: report.pdf
          if-no-files-found: error
```

<a id="activation"></a>

## Activating a Syhunt runner

The Syhunt runner is a self-hosted GitHub Actions runner that receives scan requests, runs the scans and reports the results back to GitHub.

> [!IMPORTANT]
> For security and performance, install the runner on a separate virtual machine or a dedicated physical machine.

1. Install [Git for Windows](https://gitforwindows.org/) with the default settings.
2. Install Syhunt Hybrid (`syhunt-hybrid-<version>.exe`) with the default settings.
3. On GitHub, open the repository's main page and:
   1. Click **Settings**.
   2. In the left sidebar, click **Actions**, then **Runners**.
   3. Click **New self-hosted runner**.
   4. Select the **Windows** operating system and the **x64** architecture.

   GitHub shows instructions for downloading the runner application and installing it on your machine.
4. Follow those instructions to register the runner with the repository's token. When you run `config.cmd`:
   1. Press **Enter** to add the runner to the Default group.
   2. Enter a name for the runner, for example `syhunt`.
   3. Enter a label for the runner, for example `syhunt`.
   4. The messages **√ Runner successfully added** and **√ Runner connection is good** confirm success.
   5. Press **Enter** to keep `_work` as the work folder.
   6. Choose whether to run the runner as a Windows service.
5. Run `run.cmd`. You should see:
   ```
   √ Connected to GitHub
   2022-02-04 19:50:21Z: Listening for Jobs
   ```

Syhunt can now be called from GitHub Actions workflows.

> [!NOTE]
> GitHub automatically removes a self-hosted runner that hasn't connected to GitHub Actions for more than 30 days. Each workflow run is also limited to 72 hours; runs that reach the limit are cancelled.

<a id="addyml"></a>

## Adding Syhunt to a workflow

If your repository doesn't have a workflow file yet, create one, for example `.github/workflows/syhunt.yml`.

**SAST example:** scans the repository's source code, fails the job if medium- or high-risk vulnerabilities are found, and attaches a PDF report to the job's artifacts:

```yaml
name: syhunt test
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: [self-hosted, Windows, X64]
    steps:
      - uses: actions/checkout@v2
      - run: Start-CodeScan -pfcond 'medium' -output 'report.pdf'
      - name: 'Syhunt Report'
        uses: actions/upload-artifact@v2
        if: failure()
        with:
          name: syhunt-report
          path: report.pdf
          if-no-files-found: error
```

**DAST example:** scans the live web application after deployment, fails the job if medium- or high-risk vulnerabilities are found, and attaches a PDF report to the job's artifacts:

```yaml
name: syhunt test
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: [self-hosted, Windows, X64]
    steps:
      - uses: actions/checkout@v2
      - run: Start-DynamicScan -target 'www.productionurl.com' -pfcond 'medium' -output 'report.pdf'
      - name: 'Syhunt Report'
        uses: actions/upload-artifact@v2
        if: failure()
        with:
          name: syhunt-report
          path: report.pdf
          if-no-files-found: error
```

<a id="dast"></a>
<a id="sast"></a>
<a id="passfail"></a>

## Scan functions and pass/fail conditions

Workflow steps call the Syhunt PowerShell functions. See the PowerShell guide for all parameters and more examples:

- [`Start-DynamicScan`](powershell.md#dynamicscan)
- [`Start-CodeScan`](powershell.md#codescan)
- [More examples](powershell.md#examples)
- [Pass/fail conditions](powershell.md#pfcond)

![](../images/integrations_glpassfail.png)

<a id="submitissues"></a>

## Sending vulnerabilities to GitHub Issues

Once a GitHub issue tracker is configured, you can submit a vulnerability to a project with one click.

![](../images/tracker_github.png)

See [Issue Trackers: GitHub](issue-trackers.md#tracker_github) for how to set up the tracker, and [Submitting vulnerabilities to a tracker](issue-trackers.md#tracker_submit) for how to send findings.
