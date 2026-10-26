# Azure DevOps

> **Applies to:** Syhunt Hybrid 7.2

Since version 6.9.6, Syhunt Community and Syhunt Hybrid can connect to Azure DevOps Services, Azure DevOps Server and Team Foundation Server (TFS) and scan the source code of projects for application security vulnerabilities and weaknesses.

This feature is available from the user interface and the CLI on Windows, and works with on-premises and cloud-hosted projects, both TFVC and Git. Out of the box, Syhunt supports Azure DevOps Services, Azure DevOps Server 2020 and 2019, and TFS 2018 down to 2015. As explained below, it can also be configured to connect to older TFS versions (2013, 2012 and 2010).

<a id="tfstoolssuite"></a>

For pipeline automation, see also the [PowerShell integration](powershell.md).

> [!NOTE]
> TFS support in Syhunt Community and Hybrid relies on the export tool developed by Microsoft MVP Neno Loje, freely [available from his website](https://blogs.msmvps.com/vstsblog/). It is bundled with Syhunt with the developer's permission.

<a id="languages"></a>

## Supported versions

| Version | Deployment |
|---|---|
| Azure DevOps Services 2021 | Cloud |
| Azure DevOps Server 2020 / 2019 | On-premises |
| Team Foundation Server 2018 / 2015 | On-premises |
| Team Foundation Server 2013 / 2012 / 2010 | On-premises\* |

\* Scanning projects on TFS 2010–2013 requires Microsoft Visual Studio or Microsoft Team Explorer on the same machine as Syhunt.

To scan Git repositories (not only TFVC), install [Git for Windows](https://gitforwindows.org/) on the same machine.

## Project URL formats

| Repository | Example target |
|---|---|
| Azure DevOps Services, TFVC | `https://dev.azure.com/user/projectname` |
| Azure DevOps Services, Git | `https://dev.azure.com/user/projectname/_git/projectname` |
| Azure DevOps Server | `https://myserver/tfs/projectname` |
| Azure DevOps Services / Server / TFS, by collection | `collection:https://dev.azure.com/user$/projectname` |

With the `collection:` form, Syhunt connects to the project collection at `https://dev.azure.com/user` and scans the server path `$/projectname`.

## Scanning from the user interface

In the Syhunt Hybrid Web UI (or the Community GUI), click **New Scan**, select **Project URL** and enter the project's URL, as described in [Source Code Scanning](../scanning/code-scanning.md#code_begin). This works for Azure DevOps and TFS/TFVC projects as well as Git repositories hosted in Azure DevOps. You can also schedule code scans of project URLs with the [scan scheduler](../scanning/scheduling.md).

![](../images/codescan_projecturl_azure.png)

Syhunt asks for your Azure DevOps credentials before connecting, unless you've already authenticated.

To connect to an older TFS server (2010 to 2013), choose its version on the **Advanced** tab. As noted above, this requires Visual Studio or Team Explorer on the same machine.

## Scanning from the command line

```bat
scancode https://dev.azure.com/user/projectname
scancode https://myserver/tfs/projectname
scancode collection:https://dev.azure.com/user$/projectname
```

Syhunt asks for your Azure DevOps credentials before connecting, unless you've already authenticated.

For older TFS servers, specify the version with `-tfsv`:

```bat
scancode https://myserver/tfs/projectname -tfsv:2013
scancode https://myserver/tfs/projectname -tfsv:2012
scancode https://myserver/tfs/projectname -tfsv:2010
```

See the [CLI guide](command-line.md#cli_code_windows) for more examples.

## Pipeline integration

You can automate Azure DevOps scans with:

- **PowerShell**: see the [PowerShell guide](powershell.md).
- **The CLI**: as shown above and in the [CLI guide](command-line.md#cli_code_windows).
