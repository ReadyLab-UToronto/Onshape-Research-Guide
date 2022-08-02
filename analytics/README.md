# Onshape Enterprise Analytics 
This guide aims to provide a general guideline of conducting research analysis to the design **process** in Onshape with the Enterprise Analytics. Specifically, this guide will primarily focus on using the [audit trails](https://cad.onshape.com/help/Content/audit_reports.htm?tocpath=Enterprise%7CAccessing%20Analytics%7C_____2). Visit the official Onshape help center [here](https://cad.onshape.com/help/Content/EnterpriseHelp/Content/reports.htm?tocpath=Enterprise%7CAccessing%20Analytics%7C_____0) for more information on other features of Enterprise Analytics. Before getting started with this guide, you should know the basics of Onshape [documents](https://cad.onshape.com/help/Content/introduction.htm?tocpath=Welcome%20to%20Onshape%20Help%7COnshape%20Documents%7C_____0) and [tabs](https://cad.onshape.com/help/Content/elementtabs.htm?tocpath=Welcome%20to%20Onshape%20Help%7COnshape%20Documents%7C_____1). 

Please also note that the Analytics portal is only available and accessible in an Onshape Enterprise account with granted access by the Enterprise administrator. 

## Table of content 
- [1. Introduction](#1-introduction)
- [2. Onshape environment set up](#2-onshape-environment-set-up)
- [3. Data preparation](#3-data-preparation)
- [4. Data analysis](#4-data-analysis)
- [5. Sample methodologies](#5-sample-methodologies)

## 1. Introduction 
Audit trail is one of the many features in the Onshape Enterprise Analytics portal. It is essentially a list of actions that were committed by the users in documents within the Enterprise account. These actions are recorded automatically as users access and make edits in documents, and the data entries are presented in a chronological order. 

There are several general notes for researchers and teachers to keep in mind when desigining your experiments and/or studies if you are planning to use the audit trail as a source of analysis data: 
- Although Onshape aims to provide real-time data updates to the Analytics portal, there is a time delay of roughly 30 minutes after an action is committed in an Onshape document and before the action is revealed in the audit trail. 
- To minimize the risk of missing audit trail data, try to do the following: 
    - Create the empty Onshape document before the experiment and make some random changes in the document. Make sure the changes are recorded in the audit trail before starting the experiment. 
    - Ask the participants to wait a few seconds after finish designing in the Onshape document and go back to the home page of their account (click the enterprise logo at the top left corner of the webpage) instead of closing the webpage directly. 

## 2. Onshape environment set up 
As mentioned in the previous section, it is recommended to create the empty or prepared Onshape documents for the participants before actually conducting the experiment. When setting up these documents, and designing your experiments in general, there are some tips to be considered: 
- It is always good practice to organize all the files for one set of study documents in one folder and/or project for easy file management and sharing. This allows easier data filtering during data preparation. 
- For more details on setting up and using Onshape in educational settings as an administrator of an Enterprise account, you can visit [this page](https://cad.onshape.com/help/Content/onshape_classroom.htm?tocpath=EDU%C2%A0Enterprise%7C_____1) of the Onshape help center. This information is also useful in setting up medium- to large-scale experiments. 
- Learn about the filtering options available for the audit trails and think about if your design of experiment allows efficient filtering for experimental data. 
- If applicable, you may want to come up with some naming conventions for the names of the tabs and documents, and ask the participants not to change them, for efficient analysis after the experiment. 
- After the experiment is finished, you may want to unshare the documents with the participants so that no more changes can be made to the documents and you can avoid the risk of having the documents being accidentally deleted by the participants. 

## 3. Data preparation 
To retrieve the experimental data from the Onshape Enterprise Analytics portal, there are some general steps that you may want to follow: 
1. First, make sure you have the correct permission, enabled by your Enterprise administrator, to access the analytics for all users in the account. For more information, please refer to [this page](https://cad.onshape.com/help/Content/EnterpriseHelp/Content/global_permissions.htm?tocpath=Enterprise%7CGetting%20Started%20as%20an%20Enterprise%20Administrator%7C_____2).
2. Access "Analytics" at the top bar of the webpage of your Analytics account. Under "All" $\rightarrow$ "Audit" on the left panel, access "Audit Trail". 
3. On the Audit Trail page, use all the filtering options available on the page to filter data. Generally, one audit trail should serve as one data point in your research dataset. As an example, you may want to download one audit trail per document to analyze behaviours for different design teams, or you may want to download one audit trail per user to analyze individual behaviours in a (collaborative) design process. 
4. When downloading the audit trail, the default option is to only download a maximum of 500 entries. To download all entries:  
    - Hover your mouse on top of the analytics table entries.
    - There should be three dots appearing right next to the top of the "Description" column. When you hover your mouse on the three dots, it should say "Tile actions", but NOT "Dashboard actions". 
    - Click "Download data". It is recommended to download the data in CSV format. 
    - Under the "Advanced data options" drop-down list, select "All results" for the "Number of rows to include". Otherwise, you will only get a maximum of 500 row entries for the downloaded dataset. 
    - Once you click "Download", it may take a while, depending on the size of your dataset. It is recommended to double check your filters and a few entries from the Analytics portal to make sure they are indeed what you want to specify before you download due to the large volumn of data. 
5. Once you download an audit trail, it will be named as "Audit Trail Dashboard.csv" by default. Before downloading more audit trails, you should come up with a naming convention for all the audit trail files. 
    - Preferably, you should name these files (and even the users' name in the audit trails) with a randomly generated naming scheme and store the codex in a separate file. This way, the identities of all the participants can be anonymized during the analysis to eliminate potential bias. 
6. Please note that all the audit trails can be sensitive human-subject experimental data, and they should be stored securely. 

## 4. Data analysis 
All audit trails downloaded from the Enterprise Analytics portal in CSV format should have six columns. The content in each column should be self-explanatory, but here are some notes to pay attention to: 
1. Column 1 is an index column with no column title. 
2. `Time`: the timestamp of the action in the format of `YYYY-MM-DD HH:MM:SS`. 
    - All timestamps are recorded in the Universal Coordinated Time (UTC) to avoid conflicts with timezone difference between collaborators in the same document. 
    - If you ever need to convert the CSV file to Excel, be careful with the timestamp when you try to convert the Excel sheet back to CSV format. You may lose the seconds of the timestamp and may cause programming errors when reading datetime data format. 
3. `Document`: the name of the Onshape document in its current form. 
4. `Tab`: the name of the tab that the action was recorded in. 
    - You may not be able to tell if the tab is a Part Studio or an Assembly by the name of the tab, unless you track all the name changes to the tabs from the beginning. Advising/enforing a tab naming convention for your participants may be beneficial for your research analysis. 
    - Some general actions may have `Tab` to be as `N/A` (e.g., "Close document"). 
5. `User`: the email address of the user that committed this action. 
    - For anonymizing purpose mentioned in step 5 of the previous section, you may also want to anonymize all the email addresses in the audit trails (e.g., by using the 'Find and Replace' functionality). 
6. `Description`: the description of the committed actions. 

To go through this large quantity of data entries, some general methods are: 
- Categorize all actions into a few categories for aggregate analysis/comparison 
- Focus on the timing, sequence, and/or transition of a few (design) behaviour types (e.g., browse between tabs, delete features)
- Combine the analysis of analytics data and other external research tools (e.g., eye/mouse tracking, screen recording)

## 5. Sample methodologies
Some open-sourced research methodologies that use the Onshape Enterprise Analytics can be found in [this page](/analytics/methodology.md). 