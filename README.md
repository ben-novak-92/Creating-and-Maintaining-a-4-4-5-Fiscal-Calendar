Creating and Maintaining a 4-4-5 Fiscal Calendar in Databricks (SQL)

1. Purpose
This SOP defines the process for generating and maintaining a 4-4-5 fiscal calendar table in Databricks using SQL.
A 4-4-5 calendar:
Divides each quarter into 4 weeks, 4 weeks, and 5 weeks
Produces 13 fiscal weeks per quarter
Produces 52 weeks (364 days) in standard years
Occasionally includes a 53rd week (371 days)
This calendar is used for:
Retail reporting
Operational trend analysis
Week-over-week comparability
Standardized fiscal period alignment across analytics

2. Scope
This procedure applies to:
Analytics Engineering
BI Developers
Data Platform Teams
Finance Reporting Consumers
The output is a reusable fiscal dimension table that can be joined to fact tables on calendar_date.

3. Definitions
Term
Definition
Fiscal Year (FY)
52 or 53-week reporting year
Fiscal Quarter
13-week block
Fiscal Period
Month equivalent (4-4-5 structure)
Fiscal Week
7-day block
Leap Year (Fiscal)
Year containing 53 weeks


4. Prerequisites
Databricks workspace access
SQL Warehouse or cluster
Create table permissions in target schema

5. Procedure
Step 1 — Determine Fiscal Rules
Before implementation, confirm:
Fiscal Year Start Date
Example: 2024-02-04
Must represent Day 1 of Week 1
Week Start Day
Sunday or Monday (must be consistent)
53-Week Years
Identify which fiscal years contain a 53rd week
Typically occurs every 5–6 years
Document these rules before proceeding.

Step 2 — Execute Fiscal Calendar Creation Script

6. Validation Checklist
After execution, validate:
Row Count Validation
364 rows per standard year
371 rows for 53-week years
SELECT fiscal_year, COUNT(*) 
FROM main.shared.fiscal_calendar_445
GROUP BY fiscal_year
ORDER BY fiscal_year;
Period Validation
Confirm:
12 fiscal periods per year
Periods follow 4-4-5 pattern
SELECT fiscal_year, fiscal_period, COUNT(DISTINCT fiscal_week_of_year)
FROM main.shared.fiscal_calendar_445
GROUP BY fiscal_year, fiscal_period
ORDER BY fiscal_year, fiscal_period;

7. Operational Maintenance
Annual Review (Required)
Once per year:
Confirm if upcoming year requires a 53rd week
Update leap_years list if needed
Regenerate table with additional fiscal years
Extension Procedure
To extend calendar:
Increase num_years
Update leap_years if applicable
Re-run script

8. Best Practices
Store calendar in a shared, governed schema
Apply Unity Catalog permissions
Add table comments describing fiscal logic
Do not modify manually after creation
Always regenerate via script

9. Downstream Usage
Join example:
SELECT
  f.order_id,
  c.fiscal_year,
  c.fiscal_period,
  c.fiscal_week_of_year,
  SUM(f.revenue)
FROM fact_sales f
JOIN main.shared.fiscal_calendar_445 c
  ON f.order_date = c.calendar_date
GROUP BY 1,2,3,4;

10. Governance & Ownership
Role
Responsibility
Data Engineering
Maintain calendar script
Finance
Approve fiscal rules
BI
Consume calendar table
Data Governance
Control access


11. Change Log
Version
Date
Change
1.0
Initial Release
Initial 4-4-5 Implementation


If you'd like, I can also provide:
A version that auto-calculates FY start based on “Sunday closest to Feb 1”
A Lakeflow pipeline version
A production-ready dimension table with surrogate keys and SCD support
A finance-grade version aligned to GAAP audit controls
Let me know the level of rigor you're targeting.

