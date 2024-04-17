# How to Share Your Data from Salesforce Data Cloud to Snowflake

Data sharing allows developers to share data from Salesforce Data Cloud’s data lake to systems like Snowflake with a zero-ETL (extract transform load) approach. This approach greatly reduces the time to work with Data Cloud data in an external data lake or system. 

## What is Salesforce Data Cloud?

Data Cloud is a customer data platform that has built-in connectors that bring in data from many sources, including Salesforce apps, mobile apps, websites, and legacy systems. The data that is ingested and harmonized into Data Cloud can be used to power analytics, marketing, and artificial intelligence. 

## What is Snowflake?

Snowflake is an advanced data platform that enables data storage, processing, and analytic solutions. It runs completely on cloud infrastructure, with the exception of its optional command-line clients, drivers, and connectors. Snowflake sits on public clouds like Amazon Web Services (AWS), Google Cloud Platform (GCP), and Azure, and it offers native support to many types of data, such as structured and semi-structured. Developers can move data to Snowflake using any ETL tool supported by the platform. Data analysts and data scientists can also report on data from Snowflake using the many reporting tools that can connect to Snowflake, like Tableau, for example. 

## Prerequisites

You will need either Account Admin, Sys Admin, or Security Admin permissions to perform these steps. Please note that this functionality is not yet available in the Salesforce Data Cloud 5-day Trailhead orgs at the time of writing this post. You can sign up for a 30-day Snowflake trial account. 

## Create the Integration User 

We’ll start with creating a worksheet in Snowflake by navigating to **Projects > Worksheets**, and click the the + icon to create a new SQL worksheet.

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/4506abcbe2547e16d9210b4afad84d8d25095b49/image%20(5).png)

Note: Account Admin, Org Admin, and Security Admin are on the blocked roles list by default for new security integrations. If you want to use one of these roles as your integration user, you will need to reach out to Snowflake support. Otherwise, you will need to use a different role. You can also create a custom role. For the purposes of this demo, we are using the Public role. 

On the SQL code worksheet, replace < Data Cloud Admin or Data Aware Specialist > with a name for the user. We’ll then need to enter information for each field to create the user. 

<pre language='SQL'>
CREATE OR REPLACE USER  &lt;Data Cloud Admin or Data Aware Specialist&gt; 
  PASSWORD = &lt;string&gt;
  LOGIN_NAME = &lt;string&gt;
  DISPLAY_NAME = &lt;string&gt;
  FIRST_NAME = &lt;string&gt;
  MIDDLE_NAME = &lt;string&gt;
  LAST_NAME = &lt;string&gt;
  EMAIL = &lt;string&gt;
  DEFAULT_ROLE = PUBLIC;
  </pre>

  Here is an example of what the SQL should look like to create a user:

  ![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(26).png)

  We’ll then press the **play button** in the top right-hand corner of the worksheet to run the worksheet. 

 ![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(27).png)

 ## Set up OAuth and grant access to the PUBLIC role

 Next, we’ll need to create a second SQL worksheet and paste the code block below into our worksheet. 

 <pre language='SQL'>
CREATE OR REPLACE SECURITY INTEGRATION DC_CLOUD
TYPE = OAUTH
ENABLED = TRUE
OAUTH_CLIENT = CUSTOM
OAUTH_CLIENT_TYPE = 'CONFIDENTIAL'
// Update the oauth URI callback
OAUTH_REDIRECT_URI = 'https://login.salesforce.com/services/cdpSnowflakeOAuthCallback'
OAUTH_ISSUE_REFRESH_TOKENS = TRUE
OAUTH_REFRESH_TOKEN_VALIDITY = 7776000;

// Get the client ID and secrets
select system$show_oauth_client_secrets('DC_CLOUD');

// Use the describe command to get the oauth authorization endpoint
DESC SECURITY INTEGRATION DC_CLOUD;

// Grant usage on the default role of the user being used in Data Cloud
GRANT USAGE ON INTEGRATION DC_CLOUD TO ROLE PUBLIC;

// To get the default role of the user
DESC USER codeyBear;
</pre>

Then, we run lines 1-9 to create the security integration by pressing **command + return** on a Mac or **ctrl + enter** on a Windows machine. 

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(28).png)

Next, we run line 11 to generate the client ID and secret by pressing **command +return** on a Mac or **ctrl + enter** on a Windows machine. Take note of the client ID and secret as you will need them later. 

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(29).png)

Finally, we run line 15 to grant the role access to the security integration. 

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(30).png)

## Create the data share target

In Salesforce Data Cloud, navigate to **Data Share Targets** and click **New**. Then select the Snowflake tile. 

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(31).png)

Next, we enter a label or name for our connection. For account URL we use our OAuth endpoint and remove everything after “.com.” For example, if our URL is “https://www.myurl.com/oauth/token-request,” we’d remove **“/oauth/token-request.”**

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(32).png)

Next, we enter in our client ID and secret.

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(33).png)

We should be redirected to a login page to enter a username and password. 
