# Northstar-Cybersecurity-Threats-and-Investigations

**SQL Portfolio Project** · **MySQL** · **Tech Studio Academy — Group A** · **3 weeks**

---

## 1. Project at a Glance

| Item | Description |
|---|---|
| Business case | Northstar Services, a fictional organisation concerned about failed logins, unfamiliar IP addresses and unresolved security alerts |
| Role | Junior SQL Data Analyst |
| Database | MySQL |
| Focus | Login activity, IP reputation, employee and device behaviour, and security alerts |
| Main deliverables | SQL scripts, management findings report and project presentation |

### Project objective

Use SQL to investigate security records, identify unusual login and alert patterns, and provide management with evidence-based areas for further review.


## 2. Database Overview

The project brief describes five connected data areas. The SQL script queries the following tables:

| Table | Information used in the analysis | Main purpose |
|---|---|---|
| `employees` | Employee name, department, job role and employment status | Analyse account activity by employee and department |
| `devices` | Device ID, employee ID, device type and operating system | Identify device usage and failed-login patterns |
| `login_logs` | Login ID, employee/device IDs, timestamp, IP address and login status | Analyse login volume, outcomes, timing and failure patterns |
| `ip_reputation` | IP address, country, risk level and reputation note | Assess login activity associated with IP reputation |
| `security_alerts` | Alert ID, employee/IP details, alert type, severity, status and timestamps | Review alert volumes, priority and resolution time |

## 3. Business Questions Investigated

| Workstream | Business questions |
|---|---|
| **A. Login activity** | How many login events occurred? What period is covered? What percentage succeeded or failed? Which days, hours and accounts had the most failed logins? Were repeated failures followed by a successful login? |
| **B. IP address investigation** | Which IPs generated the most login attempts and failures? What risk levels and countries are associated with login activity? Were successful logins associated with High Risk IPs? |
| **C. Employee and device analysis** | Which employees used multiple devices? Which accounts logged in outside normal working hours? Which devices and departments had the highest failed-login counts and rates? |
| **D. Security alerts** | How many alerts occurred by severity and type? How many were resolved or unresolved? Which High or Critical alerts remained open or under investigation? How long did resolution take? |
| **E. Advanced investigation** | How do employee accounts rank by failed logins? How do department failure rates compare with the company rate? How does daily activity change? Can management use a reusable daily summary? |

## 4. SQL Techniques & Concepts Applied

| SQL concept | How it is used in this project |
|---|---|
| `SELECT`, `WHERE`, `ORDER BY`, `LIMIT` | Retrieve, filter and sort security records |
| `COUNT()`, `SUM()` | Count login events, failed attempts, successful attempts and alerts |
| `GROUP BY`, `HAVING` | Summarise activity by status, day, hour, employee, IP, device, department, severity and alert type |
| `CASE` expressions | Count outcomes conditionally and calculate failure rates |
| `INNER JOIN`, `LEFT JOIN` | Combine login records with employee, device, IP reputation and alert information |
| Subqueries | Calculate proportions and support filtering against aggregate values |
| CTEs (`WITH`) | Organise multi-step analysis, including department and daily activity comparisons |
| Window functions: `LAG()` | Compare consecutive login events and daily failed-login counts |
| Window function: `DENSE_RANK()` | Rank employee accounts by failed-login count |
| Date/time functions | Extract dates, weekdays and hours; calculate alert-resolution duration with `TIMESTAMPDIFF()` |
| SQL views | Create `daily_security_summary` as a reusable daily reporting output |

## 5. Summary of Analytical Outputs

The tables below summarise the outputs produced by the SQL queries. They describe what each analysis calculates; they are **not numerical findings**.

### 5.1 Login activity summary

| Measure | Grouping / calculation | Business use |
|---|---|---|
| Total login events | Count all rows in `login_logs` | Establish overall login volume |
| Dataset coverage | Earliest and latest login timestamps | Understand the analysis period |
| Successful vs failed logins | Count by `login_status`; calculate percentage of total | Measure login outcomes |
| Failed logins by weekday | Weekday name and failed-attempt count | Identify days for closer review |
| Failed logins by hour | Login hour and failed-attempt count | Identify time-of-day patterns |
| Failed logins by employee | Employee name/ID and failed-attempt count | Identify accounts with repeated failures |
| Failure followed by success | Compare each employee's login sequence using `LAG()` | Flag sequences that may warrant review |

### 5.2 IP reputation summary

| Measure | Grouping / calculation | Business use |
|---|---|---|
| Login attempts by IP | IP address and total attempts | Identify IPs with high login volume |
| Failed attempts by IP | IP address and failed-attempt count | Identify IPs associated with repeated failures |
| Login outcomes by IP risk | Risk level and country; successful and failed counts | Compare login outcomes across IP reputation groups |
| Successful logins from High Risk IPs | Filter successful logins where risk level is `High Risk` | Identify events for further investigation |
| IP records by country | Country and record count | Summarise geographic distribution in the reputation data |
| IP records by risk level | Risk level and record count | Summarise the reputation categories represented |
| Potentially suspicious IP activity | Flag High Risk IPs or IPs with at least five failed attempts | Apply a transparent screening rule for follow-up |

### 5.3 Employee and device summary

| Measure | Grouping / calculation | Business use |
|---|---|---|
| Multiple-device usage | Employee and count of distinct device IDs | Identify accounts associated with more than one device |
| Out-of-hours activity | Employee, timestamp and login status; weekends or before 09:00 / from 17:00 | Review activity outside the script's stated time rule |
| Failed logins by device | Device type, operating system and failed count | Identify devices associated with more failures |
| Failed logins by department | Department and failed-attempt count | Compare failure volumes across departments |
| Department failure rate | Failed attempts ÷ total attempts × 100 | Compare departments while accounting for login volume |
| Employee/device failure rate | Failed attempts ÷ total attempts × 100, for groups with at least five attempts | Highlight groups with comparatively high failure rates |

### 5.4 Security alert summary

| Measure | Grouping / calculation | Business use |
|---|---|---|
| Alerts by severity | Severity and alert count | Understand the distribution of alert priority |
| Alerts by type | Alert type and alert count | Identify frequently generated alert categories |
| Resolved vs unresolved | Alert status and alert count | Monitor outstanding alerts |
| Open High/Critical alerts | Filter High or Critical alerts with Open/Investigating status | Prioritise important unresolved alerts |
| Resolution duration | `TIMESTAMPDIFF()` between alert and resolution timestamps | Measure resolution time for resolved alerts |
| Alerts by department | Join alert, login and employee records | Explore where alerts are associated with employee activity |

### 5.5 Advanced and reusable reporting

| Output | SQL approach | Business use |
|---|---|---|
| Employee failure ranking | `DENSE_RANK()` over failed-login totals | Rank accounts for review |
| Department vs company rate | CTEs and percentage calculations | Compare each department's rate with the overall rate |
| Daily change in failures | Daily aggregation plus `LAG()` | Compare failed logins with the preceding day |
| `daily_security_summary` view | `CREATE OR REPLACE VIEW` with daily totals, failures and successes | Provide a reusable daily summary for management |
| Highest-failure days | Query the view, sort by failed attempts and return the top 10 | Focus review on days with the highest failure counts |

## 6. Findings and Evidence

The SQL script contains analysis queries intended to surface the following potential findings:

| Finding area | Evidence the SQL is designed to return | Interpretation |
|---|---|---|
| Login failure patterns | Counts by weekday, hour and employee | Shows when and where failed attempts are concentrated |
| Repeated failures followed by success | Employee login sequence using `LAG()` | Identifies sequences that may deserve account-level review |
| IP reputation concerns | Successful logins joined to High Risk IP records | Identifies successful access events associated with a high-risk reputation label |
| Department differences | Department failure rates compared with the company rate | Helps distinguish high volume from a high failure proportion |
| Device patterns | Failed attempts and failure rates by device | Identifies devices for additional review |
| Unresolved priority alerts | Open/Investigating High or Critical alert records | Helps prioritise outstanding security alerts |
| Changes over time | Daily failure counts and prior-day comparisons | Highlights dates with increases in failed-login activity |

**Results note:** The uploaded materials contain the project brief and SQL script, but no exported query-result tables or completed findings report. For that reason, this README does not invent totals, percentages, employee names, IP addresses, countries, alert counts or specific peak dates. Populate those values only after running and validating the queries against the project database.

## 7. Potential Management Actions

The project brief asks for recommendations that are connected to actual findings. Once query results have been checked, the analysis can support consideration of:

1. Reviewing employee accounts with repeated failed logins, particularly sequences followed by a successful login.
2. Investigating successful logins associated with IP addresses labelled High Risk.
3. Prioritising unresolved High and Critical alerts.
4. Reviewing devices or departments with elevated failed-login rates.
5. Monitoring daily login activity using the `daily_security_summary` view.

These are possible follow-up actions. They should be included as final recommendations only where the executed query results support them.

## 8. Data and Query Limitations to Validate

The project brief explicitly requires limitations to be documented and unusual activity to be distinguished from confirmed compromise. In addition, a review of the supplied SQL script identifies items to validate before presenting final results:

| Item to validate | Why it matters |
|---|---|
| Database and table naming | The script switches between `internship`, `northstar_services` and `cybersecur_investigation`; confirm which database contains the final project tables |
| Column naming | The brief's expected field names differ in places from the names used in the SQL script, such as `login_time` vs `login_timestamp` and `resolution_status` vs `alert_status`; confirm the actual schema |
| Login-hour column | The script updates `login_hour` before the `ALTER TABLE` statement that adds it; run the column creation before updating it |
| Company failure-rate query | The company-level CTE filters on `' Failed'` with a leading space; confirm the stored status value and remove unintended whitespace if necessary |
| Department alert join | The department alert query's join condition should be checked; the `ON` clause shown in the script does not contain a complete equality condition |
| Alert status and severity values | Confirm exact stored spellings and capitalisation, because filters depend on values such as `Open`, `Investigating`, `High` and `Critical` |
| Suspicious-activity rule | The threshold of five failed attempts and the High Risk label are screening criteria in the query, not proof of malicious activity |
| Missing or unmatched IP records | A login IP without a matching reputation record cannot be assigned a reputation category from the available join |

These checks are important for ensuring that reported figures are accurate and reproducible.

## 9. Skills Demonstrated

**Technical skills**
- MySQL and SQL querying
- Data aggregation and conditional calculations
- Joins across relational tables
- Subqueries and Common Table Expressions (CTEs)
- Window functions (`LAG()`, `DENSE_RANK()`)
- Date and time analysis
- SQL views and reusable reporting
- Data validation and query troubleshooting

**Analytical skills**
- Security log analysis
- Login behaviour and failure-rate analysis
- IP reputation investigation
- Employee and device activity analysis
- Security alert monitoring
- Trend and comparative analysis
- Evidence-based interpretation and reporting

## 10. Conclusion

This project applies SQL to a practical cybersecurity investigation scenario. It brings together login logs, employee and device records, IP reputation information and security alerts to answer operational questions and identify patterns for further review.

The work demonstrates my ability to translate business questions into SQL analysis, combine data from related tables, calculate useful summaries, compare activity over time and communicate findings with appropriate attention to evidence and limitations.

---

### Project Details

| Field | Detail |
|---|---|
| Project | Cybersecurity Log Analysis & Threat Investigation |
| Case organisation | Northstar Services (fictional) |
| Role | Junior SQL Data Analyst |
| Training provider | Tech Studio Academy |
| Group | Group A |
| Database | MySQL |
| Duration | 3 weeks |

**Suggested repository contents:** `README.md`, SQL script, project brief, validated findings report, and final presentation.

