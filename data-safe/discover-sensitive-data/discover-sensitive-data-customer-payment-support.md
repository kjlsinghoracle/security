# Discover sensitive data across customer, payment, and support schemas

## Introduction

In the previous two labs, you investigated the security posture of the database and then reviewed who can access it and what they can do. You have now identified risky configuration changes and changes to privileged users and entitlements. The next question is more fundamental: What data are we actually trying to protect? Knowing that a user has access to a database does not tell you whether that access puts sensitive information at risk. To understand the potential impact of a compromised or over-privileged account, you need to know where sensitive data resides.

Manually locating sensitive information across database tables and columns can be difficult, particularly as databases grow and application schemas change. Oracle Data Safe Data Discovery helps you build an inventory of sensitive data by inspecting the actual data in your target database and its data dictionary. You specify the types of sensitive information you are interested in, and Data Safe identifies columns that contain or are related to that information.

### Scenario

Continue acting as the database security administrator from the previous labs. You have already done the following:

- Reviewed the database's configuration and established an approved security baseline
- Detected a risky configuration change
- Reviewed database users and identified changes to privileged access

Now your security team asks a different question: If one of these accounts were compromised, what sensitive information could potentially be exposed?

Your first step is to discover where sensitive data exists in the database. You will use Data Discovery to examine the \`CUSTOMER\`, \`PAYMENT\`, and \`SUPPORT\` schemas and identify sensitive columns by using the common sensitive types. In this workshop environment, the \`PAYMENT\` schema is used in place of a \`PRODUCT\` schema. You will review the results and sample data to understand what information is being protected.

When the discovery completes, the expected result is 14 sensitive columns across 3 schemas, 4 tables, and 10 sensitive types.

Estimated Lab Time: 15 minutes

### Objectives

In this lab, you will:

- Discover sensitive data in the \`CUSTOMER\`, \`PAYMENT\`, and \`SUPPORT\` schemas
- Use the common sensitive types to scan the selected schemas
- Review where sensitive information is stored
- Examine the sensitive data model and its discovery counts
- Review the 14 sensitive columns discovered by Data Safe

### Prerequisites

This lab assumes you have:

- Obtained an Oracle Cloud account and signed in to the Oracle Cloud Infrastructure Console
- Access to or prepared an environment for this workshop
- Access to a registered target database

### Assumptions

- Your data values might be different than those shown in the screenshots.
- Please ignore the dates for the data and database names. Screenshots are taken at various times and may differ between labs and within labs.
- The names of compartments, target databases, and sensitive data models might differ in your tenancy.

## Task 1: Discover sensitive data in the customer, payment, and support schemas

Navigate to the **Data discovery** landing page.

1. Select **Discover sensitive data**.

   The **Create sensitive data model** wizard opens.

2. For **Step 1 - Provide basic information**, do the following, and then select **Next**:

   - In the **Name** box, enter \`SDM_CPS\`.
   - Select your compartment, if needed.
   - In the **Description** box, enter \`Sensitive Data Model - Customer, Payment, Support\`.
   - Select the compartment for your target database, and then select the name of the target database.

   ![Provide basic information](https://github.com/kjlsinghoracle/security/blob/main/data-safe/discover-sensitive-data/images/provide-basic-information-page.png)

3. For **Step 2 - Select schemas**, wait for the schemas to be refreshed if prompted to do so. Leave **Select specific schemas only** selected. Select the \`CUSTOMER\`, \`PAYMENT\`, and \`SUPPORT\` schemas, and then select **Next**. You might need to use the right arrow button at the bottom of the page to navigate to another page.

   ![Select schemas](https://github.com/kjlsinghoracle/security/blob/main/data-safe/discover-sensitive-data/images/select-schemas-page.png)

4. For **Step 3 - Select tables for schemas**, leave **All tables** selected, and select **Next**.

   ![Select tables for selected schemas](https://github.com/kjlsinghoracle/security/blob/main/data-safe/discover-sensitive-data/images/select-tables-for-selected-schemas.png)

5. For **Step 4 - Select sensitive types**, review the common sensitive types. From the dropdown list, select **All sensitive types** and review them. Switch back to **Common sensitive types**, and then select all of the common sensitive types by selecting the **Sensitive type** check box. Select **Next**.

   ![Select all common sensitive types](https://github.com/kjlsinghoracle/security/blob/main/data-safe/discover-sensitive-data/images/select-all-common-sensitive-types.png)

6. For **Step 5 - Select discovery options**, select **Collect, display and store sample data**.

   ![Select discovery options](https://github.com/kjlsinghoracle/security/blob/main/data-safe/discover-sensitive-data/images/select-discovery-options-page.png)

7. Select **Create sensitive data model** to begin the data discovery process. Wait for the sensitive data model to be created.

   The \`SDM_CPS\` page opens.

## Task 2: Analyze the sensitive data model

Review the information about the sensitive data model.

- The **Details** tab lists general information about your sensitive data model, the target database, sensitive data information, and sensitive data counts.
- Select the respective **View details** buttons to review the schemas, sensitive types, sensitive schemas, and sensitive types discovered.

  In the workshop environment, verify the following counts:

  - **Sensitive schemas discovered:** 3
  - **Sensitive tables discovered:** 4
  - **Sensitive columns discovered:** 14
  - **Sensitive types discovered:** 10

  ![Sensitive Data Model Details tab](https://github.com/kjlsinghoracle/security/blob/main/data-safe/discover-sensitive-data/images/sensitive-data-model-details-tab.png)

Select the **Sensitive columns** tab and review the discovered sensitive columns.

- For each sensitive column, you can view its schema name, table name, column name, sensitive type, parent column, data type, sample data (if you chose to retrieve sample data and if it exists), confidence level, estimated row count, and audit records.
- Review the sample data to get an idea of what it looks like. Do not treat the sample values as production data.
- If a sensitive column was discovered because it has a relationship to another sensitive column as defined in the database's data dictionary, the other sensitive column is displayed in the **Parent column** column.

The expected 14-column discovery inventory is:

| Schema | Table | Column | Sensitive type |
| --- | --- | --- | --- |
| CUSTOMER | CUSTOMERS | CUSTOMER_ADDRESS | Full Address |
| CUSTOMER | CUSTOMERS | DATE_OF_BIRTH | Date of Birth |
| CUSTOMER | CUSTOMERS | EMAIL_ADDRESS | Email Address |
| CUSTOMER | CUSTOMERS | FIRST_NAME | First Name |
| CUSTOMER | CUSTOMERS | LAST_NAME | Last Name |
| CUSTOMER | CUSTOMERS | PHONE_NUMBER | Phone Number |
| CUSTOMER | CUSTOMERS | POSTAL_CODE | Postal Code |
| CUSTOMER | CUSTOMERS | SSN | National Identifier |
| CUSTOMER | ORDERS | SHIPPING_ADDRESS | Full Address |
| CUSTOMER | ORDERS | SHIPPING_ZIP | Postal Code |
| PAYMENT | PAYMENTS | CARDHOLDER_NAME | Full Name |
| PAYMENT | PAYMENTS | CARD_NUMBER | Card Number |
| SUPPORT | SUPPORT_TICKETS | CONTACT_EMAIL | Email Address |
| SUPPORT | SUPPORT_TICKETS | CONTACT_PHONE | Phone Number |

![Sensitive Data Model Sensitive Columns tab](https://github.com/kjlsinghoracle/security/blob/main/data-safe/discover-sensitive-data/images/sensitive-data-model-sensitive-columns-tab.png)

You may now **proceed to the next lab**.

## Learn More

- [Data Discovery Overview](https://docs.oracle.com/iaas/data-safe/doc/data-discovery-overview.html)

## Acknowledgements

- **Author** - Jody Glover, Lead Principal User Assistance Developer, Database Development
- **Contributor** - Bettina Schäumer, Lead Principal Product Manager, Oracle Database Security
- **Last Updated By/Date** - Jody Glover, August 20, 2026
