# How to Share Your Data from Salesforce Data Cloud to Snowflake

Data sharing allows developers to share data from Salesforce Data Cloud’s data lake to systems like Snowflake with a zero-ETL (extract transform load) approach. This approach greatly reduces the time to work with Data Cloud data in an external data lake or system. 

## What is Salesforce Data Cloud?

Data Cloud enables you to build a comprehensive, 360-degree view of your customers across products, services, and interactions. It is natively integrated into the Einstein 1 Platform. Data Cloud has built-in connectors that bring in data from many sources, including Salesforce apps, mobile, web, connected devices, and legacy systems. The data that is ingested and harmonized into Data Cloud can be used to power analytics, marketing, and artificial intelligence. 

## What is Snowflake?

Snowflake is an advanced data platform that enables data storage, processing, and analytic solutions. It runs completely on cloud infrastructure, with the exception of its optional command-line clients, drivers, and connectors. Snowflake sits on public clouds like Amazon Web Services (AWS), Google Cloud Platform (GCP), and Azure, and it offers native support to many types of data, such as structured and semi-structured. Developers can move data to Snowflake using any ETL tool supported by the platform. Data analysts and data scientists can also report on data from Snowflake using the many reporting tools that can connect to Snowflake, like Tableau, for example. 

## Prerequisites

You will need either Account Admin, Sys Admin, or Security Admin permissions to perform these steps. Please note that this functionality is not yet available in the Salesforce Data Cloud 5-day Trailhead orgs at the time of writing this post. You can sign up for a 30-day Snowflake trial account. 

## Create the Integration User 

We’ll start with creating a worksheet in Snowflake by navigating to Projects > Worksheets, and click the the + icon to create a new SQL worksheet.

![Alt text](https://github.com/dlarregui/snowflakedatasharing/blob/4506abcbe2547e16d9210b4afad84d8d25095b49/image%20(5).png)

Note: Account Admin, Org Admin, and Security Admin are on the blocked roles list by default for new security integrations. If you want to use one of these roles as your integration user, you will need to reach out to Snowflake support. Otherwise, you will need to use a different role. You can also create a custom role. For the purposes of this demo, we are using the Public role. 

On the SQL code worksheet, replace < Data Cloud Admin or Data Aware Specialist > with a name for the user. We’ll then need to enter information for each field to create the user. 

CREATE OR REPLACE USER  &lt;Data Cloud Admin or Data Aware Specialist&gt; 
  PASSWORD = &lt;string&gt;
  LOGIN_NAME = &lt;string&gt;
  DISPLAY_NAME = &lt;string&gt;
  FIRST_NAME = &lt;string&gt;
  MIDDLE_NAME = &lt;string&gt;
  LAST_NAME = &lt;string&gt;
  EMAIL = &lt;string&gt;
  DEFAULT_ROLE = PUBLIC;

  Here is an example of what the SQL should look like to create a user:
