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

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(34).png)

Then we are redirected to another page, where we’ll need to click Allow to allow Salesforce Data Cloud to access our Snowflake account. 

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(35).png)

We can verify that our data share target was connected successfully by checking that our status is showing as “Active,” and that our authentication status is showing “Successful.”

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(36).png)

Now, let’s share our Salesforce Data Cloud data with Snowflake. 

We first navigate to **Data Shares** and click **New.** We then enter a label and choose a data space to associate our data share to. In this case, we will just use our default data space. 

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(37).png)

Next, we choose a data lake object or data model object to share. In this case, we’ll choose to share a data lake object that is ingesting data from Google Cloud Storage (see our previous blog post for information on how to set up this connector). Finally, click **Save.**

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(38).png)

Next, we choose our data share target by clicking the **Link/Unlink Data Share Target** button. 

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(39).png)

Then, we choose our data share target and click **Save**. 

## Get the data in Snowflake

In Snowflake, we navigate to **Data Products**, then **Private Sharing**. Under **Direct Shares**, we’ll see the data share from Salesforce Data Cloud. We click the **down arrow button** to get the data. 

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(40).png)

We can rename the database and choose which roles can access the database.

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(41).png)

We click **Done** or **View Database** to view the database. Please note that your data might be brought over under a different container. 

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(42).png)

We are then able to see our database under schema and views. We can view our metadata, such as field names and types. 

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(43).png)

We can also preview our data in the database. 

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(45).png)

## Conclusion

You now know how to connect and share your data from Salesforce Data Cloud to Snowflake using a zero-ETL approach. This powerful knowledge allows you to bypass time-consuming steps and build a robust integration faster. 

