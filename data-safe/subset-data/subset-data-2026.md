# Subset data

## Introduction

In the previous labs, Data Discovery identified sensitive data across the `CUSTOMER`, `PAYMENT`, and `SUPPORT` schemas for application testing. The sensitive data model inventory was used to create a masking policy, and the pre-masking checks confirmed that the target database was ready.

The next control is data subsetting. Use the OCI Data Safe **Subset database** workflow to create a smaller, referentially consistent copy for non-production application testing. This lab keeps recent 2026 orders and then retains 10% of the rows that match the date condition. Related customer and order-item/payment rows remain aligned through the relationship settings.

Do not submit a subsetting job until you have reviewed the rule and confirmed the target database and policy.

Estimated Time: 20 minutes

### Objectives

- Open the Data Safe data subsetting overview and start the **Subset database** workflow.
- Create a subsetting policy that includes the `CUSTOMER`, `PAYMENT`, and `SUPPORT` schemas.
- Add a driving-table rule for `CUSTOMER.ORDERS`.
- Filter `ORDER_DATE >= 2026-01-01`, then retain 10% of the matching rows.
- Keep referenced ancestor rows and referencing descendant rows so referential integrity is preserved.
- Review the subsetting options and submit only when the configuration is correct.

### Prerequisites

- An Oracle Cloud account with access to Data Safe.
- The registered target database used in the discovery and masking labs (`ADB_2` in the reference screenshots).
- The previous sensitive data model, masking policy, and pre-masking checks completed.
- Database credentials available for the target database when the workflow requests them.

### Scenario

Continue acting as the database security administrator. The application team needs a recent, smaller dataset for testing customer profiles, orders, payments, and support tickets. Masking has already been planned and pre-checked. Now subset the source while preserving the relationships needed by the application.

### Task 1: Open the subset database workflow

1. Open **Data Safe** and select **Data subsetting**.
2. On the overview page, select **Subset database** in the upper-right corner.
3. In **Provide basic information**, select the database compartment and target database.
4. Enter the target database credentials when prompted. Data Safe uses them to refresh statistics, estimate reduction, and run the subset job.
5. Select an existing subsetting policy or select **Create subsetting policy**.

![Data Safe data subsetting overview](images/subsetting-overview.png)

### Task 2: Create the subsetting policy scope

1. Set the policy compartment to the workshop compartment.
2. Give the policy a descriptive name such as `Subset_SDM_CPS_2026_2026`.
3. Add a description such as `Recent 2026 customer transaction data for application testing`.
4. Use **Select schemas** when the sensitive-data-model option does not expose all three application schemas.
5. Refresh the database schemas and select `CUSTOMER`, `PAYMENT`, and `SUPPORT`.
6. Create the policy, then select it in the **Subset database** workflow.

![Create a three-schema subsetting policy](images/subsetting-schemas.png)

### Task 3: Add the recent-orders rule

1. Expand **Tables and subsetting rules** and select **Add subsetting rule**.
2. Select `CUSTOMER.ORDERS` as the driving table.
3. Select **Condition and percentage**.
4. Configure the condition:
   - Column: `ORDER_DATE`
   - Operator: `>=`
   - Value: `2026-01-01`
5. Set **Percentage of rows to retain** to `10`.
6. Keep **Keep only referenced rows** for ancestors so matching `CUSTOMER.CUSTOMERS` rows are retained.
7. Keep **Keep only referencing rows** for descendants so matching `CUSTOMER.ORDER_ITEMS` and `PAYMENT.PAYMENTS` rows remain consistent.
8. For other related tables, keep the default **Keep maximum rows** unless the test scenario requires a different policy.
9. Review the relationship graph before continuing.

The workflow applies the date condition first and the 10% retention second. It does not mean “10% of the full database”; it means 10% of the rows that satisfy `ORDER_DATE >= 2026-01-01`.

![ORDER_DATE condition and 10 percent retention](images/condition-percentage-rule.png)

### Task 4: Review subsetting options

1. In **Select subsetting options**, review unrelated-table processing, degree of parallelism, redo logging, recompilation, and statistics refresh.
2. Keep the defaults unless your target-database requirements call for a change.
3. Because masking was handled in the previous lab, leave **Data masking after subsetting** disabled for this lab unless you intentionally want to combine both operations.
4. Use the active policy details page as a reference for the options and policy state.

![Live OCI Data Safe subsetting policy details](images/subsetting-policy-details.png)

### Task 5: Review and submit

1. Open **Review and submit**.
2. Confirm the target database, policy name, selected schemas, driving table, rule condition, 10% retention, and relationship settings.
3. Confirm the estimated size reduction.
4. Submit the subsetting job only after the review is complete.
5. Monitor the work request and **Subsetting reports** until the job reaches a terminal status.
6. Verify that the subset contains the recent order population and the related rows required by the application test.

### Validation checklist

| Item | Expected configuration |
| --- | --- |
| Target database | `ADB_2` or your registered target database |
| Schemas | `CUSTOMER`, `PAYMENT`, `SUPPORT` |
| Driving table | `CUSTOMER.ORDERS` |
| Condition | `ORDER_DATE >= 2026-01-01` |
| Retention | 10% of condition-matching rows |
| Ancestors | Keep only referenced rows |
| Descendants | Keep only referencing rows |
| Masking | Disabled here; handled by the preceding masking lab |

### Learn More

- [Data Subsetting overview](https://docs.oracle.com/en/cloud/paas/data-safe/udscs/data-subsetting-overview.html)

### Acknowledgements

- Author - Jody Glover, Lead Principal User Assistance Developer, Database Development
- Contributor - Kajal Singh, Product Manager, Oracle Database Security
- Last Updated By/Date - Kajal Singh, September 21, 2026
