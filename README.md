# SuiteScript-2.1-Upgrade-Report-Toolkit

**See which NetSuite scripts still need to move to SuiteScript 2.1, all on one page.**

SuiteScript 2.1 Upgrade Report Toolkit is a free, open-source tool for NetSuite. It lists every script in your account that is not on SuiteScript 2.1, so you can see your exposure to Oracle's announced end of support for SuiteScript 1.0 before it affects you, and plan the upgrade with real numbers.

- **Free and open source** under the MIT License.
- **Runs inside your NetSuite account** as two Suitelets. There is no Foretopia service behind it.
- **Writes nothing.** It creates, changes, and stores nothing in your account.
- **No outbound calls from the report.** The optional "Generate Context" page sends the selected script file's code to NetSuite's own built-in generative AI (`N/llm`), which uses your account's LLM usage credits.


> **Ready to upgrade to SuiteScript 2.1?** Foretopia's NetSuite developers can review, convert and test the scripts your team owns, so your business processes keep running. Contact us at [info@foretopia.com](mailto:info@foretopia.com) or visit [www.foretopia.com](https://www.foretopia.com).
>
> Using this tool doesn't share anything with us; we only hear from you if you reach out.

---

## Why it exists

Most NetSuite accounts collect custom scripts over the years, written by different developers and partners. Before planning an upgrade, every admin has to answer the same questions, usually by opening scripts one at a time:

- Which scripts are still on SuiteScript 1.0, 2.0, or 2.x?
- Which ones can we change, and which belong to a bundle or SuiteApp?
- Are they still running?
- What does each one actually do?

SuiteScript 2.1 Upgrade Report Toolkit answers these in one place.

---

## What it does

### 1. The report

Open the report and it loads automatically.

<img width="3826" height="1567" alt="Screenshot 2026-10-09 at 12 07 54 AM" src="https://github.com/user-attachments/assets/b831f7ff-7582-427f-b2ad-ebb1387e4cb9" />

**Summary tiles** at the top:

- Scripts not on 2.1
- Count for each API version (1.0, 2.0, 2.x, or unknown)
- Editable scripts
- Locked files
- Scripts from bundles or SuiteApps
- Scripts with no execution history

**One row per script record whose API version is not 2.1**, with these columns:

| Column | What it shows |
| --- | --- |
| Script | Script name (linked to the script record) and script ID |
| Type | Script type, for example User Event, Client, or Scheduled |
| API Version | The SuiteScript version recorded on the script record |
| Status | Active or Inactive |
| Last Executed | The most recent execution log entry or scheduled run, with its source |
| Deployment Type | **Bundle**, **SuiteApp**, or **SuiteScript**. Hover over the value for a definition. |
| Bundle / SuiteApp | The code and name of the bundle or SuiteApp that installed the script |
| Script File | File name (linked to the file record) and folder path |
| File Locked | Locked or Unlocked |
| Download | A link to download the script file, when the file can be downloaded |
| Script File Context | A **Generate Context** button for editable files |

**Working with the table:**

- A filter sits above every column: a dropdown for columns with a few fixed values, and a text box for the others.
- Click any column header to sort by it, and click again to reverse the order.
- **Clear** resets all filters, and the count shows how many rows match ("Showing X of Y").

<img width="3795" height="195" alt="image" src="https://github.com/user-attachments/assets/dfab0bd7-9343-476b-9b24-ad5ef0cea280" />

**Deployment Type definitions** (shown on hover):

- **Bundle**: the script is part of a bundle installed in your account. The bundle publisher owns the code, its files are locked, and local changes are overwritten when the bundle updates, so the upgrade should come from the publisher.
- **SuiteApp**: the script is part of a SuiteApp. The publisher manages and updates it, and its scripts and files are read-only in your account.
- **SuiteScript**: the script was developed and deployed directly in your account, by an in-house developer or a third-party vendor. Your team owns it and can upgrade it.

### 2. Generate Context (AI script summary)

For any editable script file, **Generate Context** opens a page in a new tab that explains the script in plain language:

- **Summary**: the script's business purpose, entry points, and the main records and modules it uses.
- **Pseudo code**: a short, readable outline of its logic.
- **SuiteScript 2.1 upgrade notes**: specific items to check when moving the script to 2.1, such as SuiteScript 1.0 `nlapi` calls.
- **Source code**: the file's full source, in a section you can expand.

<img width="2280" height="1673" alt="image" src="https://github.com/user-attachments/assets/bfde324b-6148-4f82-abd1-a3597c611fa9" />

How Generate Context works:

- It uses NetSuite's built-in generative AI (`N/llm`), so no external AI service is involved.
- Each run uses your NetSuite account's LLM usage credits. The page shows this notice at the top, with a link to [Oracle's documentation](https://docs.oracle.com/en/cloud/saas/netsuite/ns-online-help/article_9123730083.html#subsect_1014032311). It also shows how many free LLM calls are left.
- It sends up to the first 30,000 characters of the file. If the file is longer, the page says so.
- It only reads. It does not change the script, the file, or anything else, and nothing is saved. Click **Regenerate** to produce new content.
- It does not run on bundle or SuiteApp files.
- AI-generated content can be wrong, so check it against the source code.

---

## Your data

| | The report | Generate Context |
| --- | --- | --- |
| Reads | Script, file, folder, and execution history metadata | The selected script file's source code, and the script records that use it |
| Writes or stores anything | No | No |
| Outbound calls | None | Only NetSuite's own generative AI service, through `N/llm` |
| Sends data to Foretopia or third parties | No | No |
| Telemetry or usage tracking | No | No |

The tool does not read transactions, customers, vendors, items, or other business data. The full statement, including every record type and field read, the modules used, and what is logged.

---

## Requirements

- NetSuite with server-side SuiteScript enabled.
- NetSuite's generative AI (`N/llm`) available in your account, if you want to use Generate Context.

## Account size and limits

NetSuite gives each Suitelet page load a budget of **1,000 governance units**. The report builds everything in one page load, so the number of scripts it can list depends on the size of your account.

**Recommended size:** up to about **2,000 scripts that are not on SuiteScript 2.1**. Scripts already on 2.1 are not loaded, so they don't count. Accounts with up to about **3,500** such scripts are expected to work, but with less headroom. Above that, the report is likely to stop with a NetSuite usage-limit error ("Unable to build the report").

These figures are estimates worked out from the code, not measured limits. Check your own account as described below.

What uses the budget, approximately:

| Part of the report | Approximate cost |
| --- | --- |
| Bundle and SuiteApp lookup | About 20 units per 100 scripts. This is the largest cost. |
| Reading the scripts | About 10 units per 1,000 scripts, plus 10 |
| Building folder paths | About 10 units per 1,000 folders in the account, plus 10 |
| Last executed dates | About 10 units per 1,000 scripts that have execution logs, plus about 15 |
| Bundle and SuiteApp names | About 10 units each for bundles and for SuiteApps. If NetSuite does not return names in one call, about 10 more per bundle or SuiteApp. |

Expected totals:

| Scripts not on 2.1 | Folders in account | Estimated units used | Status |
| --- | --- | --- | --- |
| 500 | 1,500 | about 200 | Comfortable |
| 1,000 | 3,000 | about 320 | Comfortable |
| 2,000 | 5,000 | about 560 | Comfortable |
| 3,000 | 5,000 | about 780 | Near the limit |
| about 3,900 | 5,000 | about 1,000 | At the limit |

Things that lower the limit:

- **Many bundles or SuiteApps.** If bundle names have to be looked up one at a time, each one adds about 10 units. For example, 50 bundles add about 500 units and lower the limit to about 1,500 scripts.
- **Very large File Cabinets.** The report reads every folder in the account to build file paths.

**Page size.** Each row adds about 1.5 KB to the page. With thousands of rows, the page takes longer to load, and filtering and sorting in the browser slow down.

**Check your own account.** Each time the report loads, it writes an Audit entry named "Script Version Report" to the deployment's Execution Log: "N scripts; remaining usage X". The units used are 1,000 − X. If X is under about 200, your account is close to the limit. This entry is written only when the deployment's log level is Audit or Debug. If the log level is set to Error, change it to Audit for one load to check.

**Generate Context** is not affected by account size. It runs as a separate page load for one script file at a time, with its own budget.

## Install

1. SuiteScript-2.1-Upgrade-Report-Toolkit : Install via Bundle ID NNNN (Customization > SuiteBundler > Search & Install Bundles). Install both Suitelets: the Generate Context button links to the Script File Context Suitelet and opens NetSuite's error page if it is not deployed.
2. Assign the included role to the users who need the report, or confirm that the deployments' Execute As Role is set to it.
3. Open the report from **Customization > Scripting > Script Deployments > "FTP | SL | Script Version Report"** and use the URL on the deployment record.

What gets installed:

| Component | Script ID | Deployment ID |
| --- | --- | --- |
| Report | `customscript_ftp_sl_script_ver_report` | `customdeploy_ftp_sl_script_ver_report` |
| Script File Context | `customscript_ftp_sl_script_file_context` | `customdeploy_ftp_sl_script_file_context` |

Both files are installed in the File Cabinet folder `SuiteScripts/ftp_script_upgrade_analysis`.

---

## Interpreting the results

- The report identifies scripts by the API version recorded on each script record, at the moment the page loads. It is an inventory, not a remediation plan, and it does not change anything.
- Deployment type, bundle and SuiteApp membership, and file lock status are worked out from NetSuite's bundle and SuiteApp information and each file's folder. NetSuite does not provide a direct "locked" flag.
- Last executed dates come from script execution logs, which NetSuite keeps for a limited time, and from scheduled script history. A script that ran without writing a log can show "No history".
- Deprecation dates and scope are set by Oracle and may change. Check Oracle's official release notes before acting.

## Verify what you installed

- Each release is tagged in this repository with the SHA-256 of the exact files.
- To confirm there are no outbound calls, search the source for `N/https`, `N/http`, `N/email`, or `N/sftp`. None are present.
- To confirm nothing is written, search for `N/record`, `file.create`, or `.save(`. None are present.

---

## Support

Provided as-is, with no warranty or support commitment.

For help upgrading your scripts to SuiteScript 2.1, contact Foretopia at [info@foretopia.com](mailto:info@foretopia.com) or visit [www.foretopia.com](https://www.foretopia.com).

---

NetSuite and Oracle are registered trademarks of Oracle and/or its affiliates. SuiteScript 2.1 Upgrade Report Toolkit is an independent tool published by Foretopia, LLC and is not affiliated with, endorsed by, sponsored by, or supported by Oracle or NetSuite.

Built by [Udaykumar Patil - Foretopia](https://www.foretopia.com).
