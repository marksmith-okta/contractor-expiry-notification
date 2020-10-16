<!-- Copy and paste the converted output. -->

<!-----
NEW: Check the "Suppress top comment" option to remove this info from the output.

Conversion time: 0.717 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β29
* Thu Oct 15 2020 22:17:56 GMT-0700 (PDT)
* Source doc: readme.md
----->



# Contractor Expiry Notification


## <span style="text-decoration:underline;">Overview</span>

Many organizations utilize contractors as well as full time employees. A contractor would usually have a contract expiry date. That is the date at which their current contract is due to expire. Certain people within the organization, like the contractors manager, will need to be notified ahead of the expiry date, so they can potentially renew the employees contract.

This workflow reads a custom attribute on the users Okta profile. This custom attribute holds the users current contract expiry date. Based on a set number of days, a future date is calculated. A list of all users that have a matching contract expiry date is compiled and emailed to a pre-configured list of recipients.

This workflow also demonstrates how static configuration values, like days, timezone and email recipients, can be externalized into a workflow table and be read in at runtime. This makes modifications to the workflow simple and easy, where changes only involve updating the respective configuration table.


### Workflow Summary

Here is a summary of the flows included in the flowpac:

**1.0 - Contractor Expiry Notification**

This is the parent flow and is initiated via a flow schedule. This flow will check to see if any users have a contract expiry date of a set number of days into the future. The exact number of days is set in the configuration table. For any users found, an email will be generated and sent to the configured recipients.

This parent flow will call the following child flows:


    1.1 - Initialize


    1.2 - Process Contractor


    1.3 - Send Notification Emails

**1.1 - Initialize**

This child flow reads configuration data from the configuration table and also initializes the contractor-list table prior to processing. The flow will return configuration values for the number of days, current time zone and email address list, to the parent flow.

**1.2 - Process Contractor**

This child flow is called within a loop, once for each user found. The flow formats the user’s first name, last name and email and appends it onto the contractor-list table.

**1.3 - Send Notification Emails**

This child flow will process any users that have been stored on the contractor-list table by the previous child flow. If at least one user exists on the contractor-list table, then a HTML formatted email will be sent to the email address list set in the configuration table.

<span style="text-decoration:underline;">Before you get Started / Prerequisites</span>

Before you get started, here are the things you’ll need:



*   Access to an Okta tenant with Okta Workflows enabled for your org 
*   Access to the tenants profile editor so a custom attribute can be added to the default Okta profile
*   Access to the tenants users, so the custom attribute can be populated
*   Access to an account for Office 365 Mail

<span style="text-decoration:underline;">Setup Steps</span>

Please follow these step-by-step instructions to set up this workflow.



1. Within your Okta tenants administration console, under Profile Editor, open the Okta default profile and add the following custom attribute:
    1. Data Type: string
    2. Display Name: Contract End Date
    3. Variable name: contractEndDate
2. Within the workflows console, create a new folder and import the workflow flopack.
3. Open the workflow folder and select Tables and open the configuration table. Then import the sample configuration data and modify where appropriate. The configuration data consists of the following:
    4. addressList - A comma delimited list of email addresses
    5. timezone - Your current timezone as per the list here: [https://en.wikipedia.org/wiki/List_of_tz_database_time_zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)
    6. days - The number of days into the future that the workflow will calculate the contract expiry date
4. Within the flow titled 1.0 - Contractor Expiry Notification, update the Okta card to use your Okta connector.
5. Within the flow titled 1.3 - Send Notification Emails, update the Office 365 Mail card to use your Office 365 connector.

<span style="text-decoration:underline;">Testing this Flow</span>

This is how to test the flow.



1. Ensure the configuration table value for addressList contains an address that you can receive emails for testing.
2. Set the value of the custom attribute for at least one user. The date format must match the following: yyyy-MM-DD. Set the value to be a set number of days into the future. EG. 30 days. (This value must match the setting on the configuration table for days)
3. Initiate the parent flow (1.0 - Contractor Expiry Notification) by clicking the Test button.
4. If the flow finds at least one user with a contract expiry date matching the set number of days, then an email will be sent to the configured recipients.

<span style="text-decoration:underline;">Limitations & Known Issues</span>

There are no limitations or known issues, at this time.
