# PowerShell

> **Applies to:** Syhunt Hybrid 6.9.17

You can run Syhunt scans from PowerShell scripts, which makes it easy to bring Syhunt Dynamic, Syhunt Code and Syhunt Mobile into your continuous delivery pipeline and other automation. The Syhunt Hybrid installer adds a **Syhunt PowerShell module** on Windows, with functions to start scans, retrieve reports and results, and perform pass/fail testing.

![](../images/integrations_powershell.png)

## Adding Syhunt to a script

Insert code like this at the appropriate point in your PowerShell script:

<a id="examples"></a>

```powershell
# SAST: scan the current directory/repository
Start-CodeScan -pfcond "medium"

# SAST: scan a remote repository
$MyProject = @{
  target = 'https://github.com/syhunt/vulnphp.git';
  branch = 'main';
  pfcond = 'medium';
  output = 'report.pdf'
}
Start-CodeScan @MyProject

# DAST: scan a URL
$MyWebsite = @{
  target = 'https://www.somewebsite.com';
  pfcond = 'medium';
  output = 'report.pdf'
}
Start-DynamicScan @MyWebsite
```

<a id="examplesend"></a>
<a id="dynamicscan"></a>

## `Start-DynamicScan`

Launches a Syhunt Dynamic scan.

| Parameter | Description |
|---|---|
| `target` **(required)** | The URL to scan, for example `http://www.somesite.com`. |
| `huntmethod` | The [hunt method](../scanning/hunt-methods.md) to use. If omitted, the default method is used. |
| `pfcond` | Makes the script exit with a failure code when a condition is met. See [Pass/fail conditions](#pfcond). |
| `tracker` | The name of an existing tracker, or a tracker defined on the fly, that will receive a summary of the vulnerabilities found. See [examples](issue-trackers.md#tracker_submit). |
| `output` | Output file name, for example `report.pdf` or `report.html`. |
| `outputex` | A second output file name, for example `export.json`. |
| `verbmode` | `$false` by default. Set to `$true` for verbose output (beyond errors and basic information). |
| `genrep` | `$true` by default. Set to `$false` to skip generating an output file. |
| `redirIO` | `$true` by default. Set to `$false` to stop redirecting the scanner's input and output to the console. |
| `timelimit` | Maximum scan time, for example `1d`, `3h`, `2h30m`, `50m`. The scan is aborted when the limit is reached. No limit by default. |

`output` and `outputex` accept every output format Syhunt supports. Files are saved to the current working directory unless you give a full path.

```powershell
# Example 1: one line
Start-DynamicScan -target 'https://www.somewebsite.com' -pfcond 'fail-if:risk=mediumup'

# Example 2: parameters in a hashtable
$MyWebsite = @{
  target = 'https://www.somewebsite.com';
  pfcond = 'medium';
  output = 'report.pdf'
}
Start-DynamicScan @MyWebsite
```

<a id="dynamicscanend"></a>
<a id="codescan"></a>

## `Start-CodeScan`

Launches a Syhunt Code scan. All parameters are optional.

| Parameter | Description |
|---|---|
| `target` | The URL of a repository, or a local source code folder or file. If omitted, the current working directory is scanned. |
| `branch` | The branch to scan. If omitted, the Git client fetches the default branch. |
| `huntmethod` | The [hunt method](../scanning/hunt-methods.md) to use. If omitted, the default method is used. |
| `pfcond` | Makes the script exit with a failure code when a condition is met. See [Pass/fail conditions](#pfcond). |
| `tracker` | The name of an existing tracker, or a tracker defined on the fly, that will receive a summary of the vulnerabilities found. See [examples](issue-trackers.md#tracker_submit). |
| `output` | Output file name, for example `report.pdf` or `report.html`. |
| `outputex` | A second output file name, for example `export.json`. |
| `verbmode` | `$false` by default. Set to `$true` for verbose output. |
| `genrep` | `$true` by default. Set to `$false` to skip generating an output file. |
| `redirIO` | `$true` by default. Set to `$false` to stop redirecting the scanner's input and output to the console. |
| `timelimit` | Maximum scan time, for example `1d`, `3h`, `2h30m`, `50m`. No limit by default. |

```powershell
# Example 1: scan the current directory/repository
Start-CodeScan -pfcond "medium"

# Example 2: scan a remote Git repository
Start-CodeScan -target "https://github.com/someuser/somerepo.git" -huntmethod "normal" -pfcond "medium"

# Example 3: scan an Azure DevOps Services project
Start-CodeScan -target "https://dev.azure.com/user/projectname" -huntmethod "normal" -pfcond "medium"

# Example 4: scan a specific local folder
Start-CodeScan -target "C:\www\" -huntmethod "normal" -pfcond "medium"
```

<a id="codescanend"></a>
<a id="pfcond"></a>

## Pass/fail conditions

| Condition | Fails if… |
|---|---|
| `high` or `fail-if:risk=high` | a High-risk vulnerability or threat is found. |
| `medium` or `fail-if:risk=mediumup` | a Medium- or High-risk vulnerability or threat is found. |
| `low` or `fail-if:risk=lowup` | a Low-, Medium- or High-risk vulnerability or threat is found. |

## Troubleshooting

**"Script.ps1 cannot be loaded because running scripts is disabled on this system"**

Start PowerShell as an administrator and allow scripts to run:

```powershell
Set-ExecutionPolicy RemoteSigned
```

**"The term 'Start-CodeScan' is not recognized…"**

The Syhunt module isn't loaded. Run `Import-Module Syhunt` and call the function again.

<a id="autoload"></a>

## Loading the module automatically

On Windows 10 and later, the Syhunt module loads automatically.

On Windows 7, either run `Import-Module Syhunt` before calling the Syhunt functions, or add it to your PowerShell profile so it loads every time:

1. Go to your PowerShell profile folder. By default the profile is:
   ```
   C:\Documents and Settings\User\My Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
   ```
2. Create the `Microsoft.PowerShell_profile.ps1` file.
3. Add the line `Import-Module Syhunt` and save the file.
