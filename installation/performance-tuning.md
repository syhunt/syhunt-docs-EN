# Performance Tuning

## Dealing with long DAST scans

Scanning a website that generates a large amount of dynamic content can take a long time with Syhunt Dynamic. This isn't a bug or a limitation, but a consequence of a design choice: Syhunt Dynamic does **not** limit how deep it goes when mapping a site's structure and entry points, because the goal is always to test the target fully. Some sites, however, are extremely large or generate a practically infinite number of links, which becomes a problem when a scan must finish within a set time.

We consider that a DAST scan against a site with heavily dynamic content should take no more than **three hours**. To get there, Syhunt Dynamic offers:

- **Incremental scanning**, which shortens repeat scans of the same target.
- **A large and growing set of built-in rules** that speed up scans of such sites. New rules ship with Syhunt updates every week.

### DAST optimization

When the built-in rules aren't enough for a particular website, a Syhunt expert can analyze it on request and add custom rules. We call this process *DAST optimization*; it usually takes just a few hours.

The new rules speed up mapping and guide the scanner toward the relevant areas and entry points of the site, skipping redundant dynamic content. No attacks are launched against the website during optimization, so there's no risk to its integrity or availability. When the work is done, a new Syhunt release containing the new rules is made available.

**To request it, [open a support ticket](../support/reporting-bugs.md#supportticketcreation) listing the target URLs that need DAST optimization.**

## Complementing DAST with SAST

For websites with heavily dynamic content, Syhunt recommends also scanning the source code with [Syhunt Code](../scanning/code-scanning.md). Static analysis covers code paths that are hard to reach by crawling.

## Speeding up code scans

Real-time antivirus protection can slow down source code scans considerably. See [Preventing antivirus interference in pipelines](../scanning/code-scanning.md#advanced_pipeline).
