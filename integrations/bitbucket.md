# Bitbucket

> **Applies to:** Syhunt Hybrid 7.2

**Contents**

- [Introduction](#intro)
- [Activating a Syhunt runner](#activation)
- [Adding Syhunt to your pipeline](#addyml)
- [Scan functions and pass/fail conditions](#dast)
- [Bitbucket Issues](#submitissues)

<a id="intro"></a>

## Introduction

You can launch Syhunt scans from a Bitbucket Pipelines file, bringing Syhunt Dynamic, Syhunt Code and Syhunt Mobile into your continuous delivery pipeline. Runners are supported on both Windows and Linux.

The following pipelines scan the repository's source code, fail the step if medium- or high-risk vulnerabilities are found, and attach a vulnerability report to the step's artifacts.

**Windows runner:**

```yaml
pipelines:
  default:
    - step:
        runs-on:
          - self.hosted
          - windows
        script:
          - Start-CodeScan -pfcond 'fail-if:risk=mediumup' -output 'report.pdf'
        artifacts:
          - report.pdf
```

**Linux runner:**

```yaml
pipelines:
  default:
    - step:
        runs-on:
          - self.hosted
          - linux.shell
        script:
          - ${HOME}/syhunt-hybrid/carbon/scancode ${PWD} -pfcond:fail-if:risk=mediumup -rout:${PWD}/report.html
        artifacts:
          - report.html
```

<a id="activation"></a>

## Activating a Syhunt runner

The Syhunt runner is a self-hosted Bitbucket runner that receives scan requests, runs the scans and reports the results back to Bitbucket.

> [!IMPORTANT]
> For security and performance, install the runner on a separate virtual machine or a dedicated physical machine.

1. Install Git with the default settings:
   - **Windows:** [Git for Windows](https://gitforwindows.org/).
   - **Linux:** the `git` package from your distribution's package manager.
2. Install Syhunt Hybrid with the default settings (`syhunt-hybrid-<version>.exe` on Windows, `syhunt-hybrid-<version>.jar` on Linux).
3. Run `java -version` in a Command Prompt or terminal and check that Java 22.0.2 or later is installed. If not, install [JDK 22 or later](https://www.oracle.com/java/technologies/downloads/).
4. On Bitbucket, open the repository and:
   1. Click **Repository settings** in the left sidebar.
   2. Scroll down to **Pipelines** and click **Runners**.
   3. Click **Add runner**. Under **System and architecture**, select **Windows (64-bit)**, or **Linux Shell** if Syhunt is installed on Linux.
   4. Give the runner a name (for example, `Syhunt`) and a label (for example, `syhunt`), then click **Next**.

   Bitbucket shows instructions for downloading the runner application and installing it on your machine.
5. Follow those instructions to activate the runner, then click **Next**.

Syhunt can now be called from your Bitbucket pipelines.

<a id="addyml"></a>

## Adding Syhunt to your pipeline

If your repository doesn't have a pipeline file yet, create `bitbucket-pipelines.yml` in its root.

### SAST

Scans the repository's source code, fails the step if medium- or high-risk vulnerabilities are found and attaches a report to the step's artifacts.

**Windows:**

```yaml
pipelines:
  default:
    - step:
        runs-on:
          - self.hosted
          - windows
        script:
          - Start-CodeScan -pfcond 'fail-if:risk=mediumup' -output 'report.pdf'
        artifacts:
          - report.pdf
```

**Linux:**

```yaml
pipelines:
  default:
    - step:
        runs-on:
          - self.hosted
          - linux.shell
        script:
          - ${HOME}/syhunt-hybrid/carbon/scancode ${PWD} -pfcond:fail-if:risk=mediumup -rout:${PWD}/report.html
        artifacts:
          - report.html
```

### DAST

Scans the live web application after deployment, fails the step if medium- or high-risk vulnerabilities are found and attaches a report to the step's artifacts.

**Windows:**

```yaml
pipelines:
  default:
    - step:
        runs-on:
          - self.hosted
          - windows
        script:
          - Start-DynamicScan -target 'www.productionurl.com' -pfcond 'medium' -output 'report.pdf'
        artifacts:
          - report.pdf
```

**Linux:**

```yaml
pipelines:
  default:
    - step:
        runs-on:
          - self.hosted
          - linux.shell
        script:
          - ${HOME}/syhunt-hybrid/carbon/scanurl www.productionurl.com -pfcond:fail-if:risk=mediumup -rout:${PWD}/report.html
        artifacts:
          - report.html
```

<a id="dast"></a>
<a id="sast"></a>
<a id="passfail"></a>

## Scan functions and pass/fail conditions

| | Windows runner | Linux runner |
|---|---|---|
| Dynamic scans | [`Start-DynamicScan`](powershell.md#dynamicscan) (PowerShell) | [`scanurl`](command-line.md#cli_dynamic_begin) |
| Code scans | [`Start-CodeScan`](powershell.md#codescan) (PowerShell) | [`scancode`](command-line.md#cli_code_begin) |
| Pass/fail conditions | [PowerShell](powershell.md#pfcond) | [CLI](command-line.md#pfcond) |

More Windows examples are in the [PowerShell guide](powershell.md#examples).

![](../images/integrations_glpassfail.png)

<a id="submitissues"></a>

## Bitbucket Issues

Integration with Bitbucket Pipelines is available now. Integration with Bitbucket Issues is planned for a future release.
