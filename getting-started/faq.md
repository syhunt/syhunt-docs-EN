# FAQ

> **Applies to:** Syhunt Hybrid 7.2

<a id="dynamic_begin"></a>

## Syhunt Dynamic

**How long does Syhunt Dynamic take to run all the tests?**

It depends on how many pages and applications the website contains, and on the scan method you choose. After crawling, the web application checks are usually the longest stage, and their duration grows with the size of the site.

**Can I reopen a previous scan and generate its reports again?**

Yes. Open **Scans** from the menu, click the scan in the list, then click **View Report**, or **Export** to save the data in a specific format.

**Is there a list of the tests performed by the current version?**

Yes. Open **Help → Vulnerability Checks**, or see [Vulnerability Checks](../reference/vulnerability-checks.md).

**Can any of the tests crash the target host?**

Some denial-of-service checks may crash the target. You can disable them by choosing a scan method without DoS tests, such as **Complete Scan, No DoS** (see [Hunt Methods](../scanning/hunt-methods.md)).

**Does Syhunt Dynamic have problems with personal firewalls?**

You'll need to allow Syhunt to make outbound connections. Some software firewalls also cope poorly with heavy traffic, so we don't recommend running a personal firewall on the same machine as Syhunt.

If the scanning machine runs a firewall with outbound filtering, try disabling it. We have occasionally seen firewalls silently block a program's connections without asking the user first.

**Can Syhunt scan ports 21 (FTP) or 23 (Telnet)?**

No. Syhunt Dynamic is not a general-purpose network scanner; it specializes in web applications and web APIs.

<a id="dynamic_end"></a>

## Product names

**What happened to Sandcat Platinum, TrustSight and Collapse?**

Sandcat Platinum was renamed Sandcat Pro and later became Syhunt Dynamic. The TrustSight and Collapse technologies were merged into Syhunt Dynamic.
