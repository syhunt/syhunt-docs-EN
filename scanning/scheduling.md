# Scheduling Scans

> **Applies to:** Syhunt Hybrid 7.2

<a id="begin"></a>
<a id="scheduling_begin"></a>

## How to schedule a scan

1. [Enable the Web UI](../installation/web-ui.md#firststeps), if you haven't done so yet.
2. In the Syhunt Web UI, click **New → Scheduled Scan**.<br>![](../images/scheduler_new.png)
3. The **Schedule New** screen opens.
4. On the **General** tab, enter a name for the scheduled scan (for example, `MyScan`), fill in the target details and choose a scan method.
5. On the **Report** tab, set the report generation options.
6. On the **Schedule** tab, define when the scan should run.<br>![](../images/scheduler_scheduletab.png)
7. Click **Save**.
8. Confirm that the new schedule appears in the list of scheduled scans at `https://127.0.0.1:8018/syhunt/?n=scanschedule`.

## Emailing reports

**1. Create an Email connector**

1. Click **Connectors** in the top menu of the Web UI.<br>![](../images/menu_connectors.png)
2. Click **Add**. The **New Connector** screen opens.
3. Enter a name for the connector (for example, `MyEmail`).
4. Set **Type** to **Email**.
5. Click **Create** and, after the success message, **Close**.

**2. Configure the connector**

1. Click the connector you just created. Its preferences screen opens.
2. Enter the sender and recipient email addresses.
3. Enter the SMTP host and authentication credentials.
4. Click **Save**.

**3. Attach the connector to a scheduled scan**

1. Open **Scans** from the top menu and click **Scheduled Scans**.
2. Click the scheduled scan whose reports you want emailed. Its preferences dialog opens.
3. On the **Notifications** tab, select **Email the report when the scan is finished**.<br>![](../images/scheduler_emailtab.png)
4. Choose your email connector from the list (`MyEmail` in the screenshot above).
5. Click **Save**.

## Reviewing results

You can review the results of past and running scans at any time: open the Syhunt Hybrid home page and click **Scans** in the top menu.


<a id="end"></a>
