# How to Share Your Data from Salesforce Data Cloud to Snowflake

Data sharing allows developers to share data from Salesforce Data Cloud’s data lake to systems like Snowflake with a zero-ETL (extract transform load) approach. This approach greatly reduces the time to work with Data Cloud data in an external data lake or system. 

In this blog post, we’ll discuss how to use data sharing to share data from Data Cloud to Snowflake. 

## What is Salesforce Data Cloud?

Data Cloud enables you to build a comprehensive, 360-degree view of your customers across products, services, and interactions. It is natively integrated into the Einstein 1 Platform. Data Cloud has built-in connectors that bring in data from many sources, including Salesforce apps, mobile, web, connected devices, and legacy systems. The data that is ingested and harmonized into Data Cloud can be used to power analytics, marketing, and artificial intelligence. 

## What is Snowflake?

Snowflake is an advanced data platform that enables data storage, processing, and analytic solutions. It runs completely on cloud infrastructure, with the exception of its optional command-line clients, drivers, and connectors. Snowflake sits on public clouds like Amazon Web Services (AWS), Google Cloud Platform (GCP), and Azure, and it offers native support to many types of data, such as structured and semi-structured. Developers can move data to Snowflake using any ETL tool supported by the platform. Data analysts and data scientists can also report on data from Snowflake using the many reporting tools that can connect to Snowflake, like Tableau, for example. 

## Prerequisites

You will need either Account Admin, Sys Admin, or Security Admin permissions to perform these steps. Please note that this functionality is not yet available in the Salesforce Data Cloud 5-day Trailhead orgs at the time of writing this post. You can sign up for a 30-day Snowflake trial account. 

## Create the Integration User 

We’ll start with creating a worksheet in Snowflake by navigating to Projects > Worksheets, and click the the + icon to create a new SQL worksheet.

![Alt text]([image (5).png](https://github.com/dlarregui/snowflakedatasharing/blob/main/image%20(5).png?raw=true))
