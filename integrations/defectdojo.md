# DefectDojo

> **Applies to:** Syhunt Hybrid 7.2

![](../images/defectdojo_screenshot.png)

[DefectDojo](https://www.defectdojo.org/) automates application security vulnerability management. It streamlines security testing with features such as importing third-party findings, merging and de-duplication, Jira integration, templating, report generation and security metrics.

Since version 6.9.17, Syhunt can connect to a DefectDojo server's API and submit SAST or DAST results automatically. Alternatively, Syhunt can generate a compatible report that you import into DefectDojo manually.

## Submitting results automatically

**1. Create a DefectDojo connector**

1. Click **Connectors** in the Web UI's top menu.<br>![](../images/menu_connectors.png)
2. Click **Add**. The **New Connector** screen opens.
3. Enter a name for the connector (for example, `MyDefectDojo`).
4. Set **Type** to **DefectDojo**.
5. Click **Create** and, after the success message, **Close**.

**2. Configure it**

1. Click the connector in the list. Its preferences screen opens.
2. Enter the DefectDojo server URL, for example `https://<IP>/`.
3. Enter your API token.
4. Click **Save**.

**3. Test it**

Click the connector in the list and choose **Run Test**. If everything is set up correctly, a test scan appears at `https://<server>/engagement/active`. Otherwise, an error message suggests what to fix.

**4. Attach it to your scans**

- **CLI:** pass `-tk:<connectorname>`.
- **Scheduled scans:** in the scheduled scan's preferences, on the **Notifications** tab, select the connector under **Issue Tracker #1** or **#2**.

### Creating the connector from the CLI

```lua
-- Add a DefectDojo tracker
scancore -tracker:add
-- Enter the type (DefectDojo) and a name for the tracker, then press Enter.

-- Configure it
scancore -tracker:set to:mytrackername -key:api.url -v:"https://serverurl/"
scancore -tracker:set to:mytrackername -key:auth.token.encrypted -vsecret

-- Test it
scancore -tracker:send -tid:TEST -to:mytrackername

-- Optional settings
scancore -tracker:set to:mytrackername -key:scan.minseverity -v:Info
scancore -tracker:set to:mytrackername -key:scan.testtitle -v:"Syhunt Scan"
scancore -tracker:set to:mytrackername -key:scan.engagementname -v:"Syhunt Hybrid Scan"
scancore -tracker:set to:mytrackername -key:scan.producttypename -v:"Research and Development"
scancore -tracker:set to:mytrackername -key:scan.productname -v:Syhunt
scancore -tracker:set to:mytrackername -key:scan.autocreatecontext -v:true
scancore -tracker:set to:mytrackername -key:scan.active -v:true
scancore -tracker:set to:mytrackername -key:scan.verified -v:true
scancore -tracker:set to:mytrackername -key:scan.closeoldfindings -v:false
scancore -tracker:set to:mytrackername -key:scan.skipduplicates -v:false
scancore -tracker:set to:mytrackername -key:scan.pushtojira -v:false
```

Then pass the tracker to PowerShell or the CLI:

```powershell
# PowerShell: existing tracker with its saved settings
Start-CodeScan -pfcond "medium" -tracker "mytrackername"

# PowerShell: existing tracker, overriding settings for this run only
Start-CodeScan -pfcond "medium" -tracker "mytrackername?productname=Syhunt###autocreatecontext=true"

# PowerShell: tracker defined on the fly
Start-CodeScan -pfcond "medium" -tracker "?app=defectdojo###apiurl=https://serverurl/###token=YOURTOKEN###productname=Syhunt###testtitle=Syhunt Scan###autocreatecontext=true"
```

From the CLI, use `-tk` instead of `-tracker`:

```
scancode <target> -pfcond:medium -tk:"mydefectdojotrackername?productname=Syhunt###autocreatecontext=true"
```

> [!NOTE]
> In `-tk` and `-tracker`, preferences must use their [short names](issue-trackers.md#shortnames).

## Generating a compatible report

To import results manually, have Syhunt generate a DefectDojo-compatible report by giving the export file name the double extension `.sast.json` or `.dast.json`. When importing the file into DefectDojo, choose the **GitLab SAST** or **GitLab DAST** format.

| Scan | Example |
|---|---|
| SAST | `scancode <target> -xout:anyfilename.sast.json` |
| DAST | `scanurl <target> -xout:anyfilename.dast.json` |

- **GitHub, GitLab and PowerShell:** pass the file name in the scan function's `outputex` parameter.
- **Jenkins:** pass the file name in the scan function's `outFilename` parameter.

> [!NOTE]
> DefectDojo currently documents only GitLab SAST support. DAST import may have [some limitations](https://github.com/DefectDojo/django-DefectDojo/issues/5863).
