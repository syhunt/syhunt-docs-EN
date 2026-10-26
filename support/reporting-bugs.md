# Reporting Bugs and Getting Support

> **Applies to:** Syhunt Hybrid 6.8.20

**Contents**

- [Long DAST scans](#lenghtyscans)
- [Reporting a false positive](#falsepositivereport)
- [Reporting a false negative](#falsenegativereport)
- [Reporting a crawling issue](#crawlingissuereport)
- [Reporting other bugs](#bugreport)
- [Creating a debug log](#debuglogcreation)
- [Opening a support ticket](#supportticketcreation)

<a id="lenghtyscans"></a>

## Long DAST scans

Scanning a website with a large amount of dynamically generated content can take a long time. See [Performance Tuning](../installation/performance-tuning.md) to understand why and how it can be addressed.

<a id="falsepositivereport"></a>

## Reporting a false positive

A *false positive* is a finding that, after manual review, turns out not to be a vulnerability or weakness. To report one so it can be fixed:

1. Describe each false positive in as much detail as possible.
2. [Create a debug log](#debuglogcreation).
3. [Open a support ticket](#supportticketcreation) and attach the debug files and your description.

<a id="falsenegativereport"></a>

## Reporting a false negative

A *false negative* is a vulnerability that exists in the target website or codebase but wasn't detected. To report one so it can be fixed:

1. Describe the vulnerability type (for example, SQL injection).
2. **Code scans:** include the **relevant fragments** of source code that contain the undetected vulnerability.
3. **Dynamic scans:** describe the case in as much detail as possible, including:
   - the affected URL, if any;
   - the steps you used to confirm the vulnerability;
   - for injection flaws, the affected parameter, the injected data and a sample of the application's response.
4. [Create a debug log](#debuglogcreation).
5. [Open a support ticket](#supportticketcreation) and attach the debug files, your description and any code fragments.

<a id="crawlingissuereport"></a>

## Reporting a crawling issue

1. Tell us whether the target URL is behind a barrier such as login form authentication. This helps us review the procedure, configuration or method used and suggest or make adjustments to the tool.
2. If the URL that isn't crawled properly is reachable, send it to us with test credentials (if needed), so we can test it directly from our lab.
3. If that isn't possible, [create a debug log](#debuglogcreation).
4. [Open a support ticket](#supportticketcreation) and attach the debug files and a description of the issue.

<a id="bugreport"></a>

## Reporting other bugs

1. Describe the problem in as much detail as possible, so we can investigate and reproduce it in our lab.
2. Take a screenshot of the problem.
3. [Create a debug log](#debuglogcreation).
4. [Open a support ticket](#supportticketcreation) and attach the debug files, your description and the screenshot.

<a id="debuglogcreation"></a>

## Creating a debug log

1. Open the sessions screen from **Menu → Past Sessions**.

   ![](../images/pastsessions.jpg)

2. Right-click the session you want to debug and choose **Debug → Export Full Data** to save the file.

   ![](../images/session_debuglog.png)

This creates a debug file with the `.sse` extension. Send it to Syhunt for analysis.

<a id="supportticketcreation"></a>

## Opening a support ticket

A support ticket is the best way to report a bug or ask for help.

1. Go to the [support ticket area](https://syhunt.ladesk.com/my_tickets#My-Tickets) and click **Submit Ticket**.
2. If you don't have an account yet, click **Register**, enter a valid email address and your name, and click **Register**. Check your email for your login details, then go back to the login page, sign in and click **Submit Ticket**.
3. Fill in the form: write your message, attach any files and click **Send**.

A Syhunt support agent will review the issue, and you'll receive a copy of every reply by email. Expect a reply within 1–2 business days. You can follow the status of your tickets under [My Tickets](https://syhunt.ladesk.com/my_tickets#My-Tickets) and reply by email or from the tickets area.
