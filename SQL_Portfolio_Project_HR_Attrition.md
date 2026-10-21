Employee Attrition Analysis

SQL Portfolio Project by Zohaib Muallim

Overview

I analysed the IBM HR Analytics Employee Attrition dataset using SQL to identify which factors are most strongly associated with employees leaving the company, and what an HR team could act on based on the findings.

Tool used: SQLite (browser based SQL environment)
Dataset: IBM HR Analytics Employee Attrition dataset (1,470 employee records covering department, role, pay, satisfaction, overtime, tenure and attrition status)
1. Attrition Rate by Department
sql
SELECT
    Department,
    COUNT(*) AS TotalEmployees,
    SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS EmployeesLeft,
    ROUND(100.0 * SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 1) AS AttritionRatePercent
FROM ibm_hr
WHERE Department != 'Department'
GROUP BY Department
ORDER BY AttritionRatePercent DESC;

Finding: Sales and Human Resources show notably higher attrition (20.6% and 19.0%) than Research & Development (13.8%), despite R&D being by far the largest department (961 employees). Attrition is not evenly distributed across the business, it is concentrated in specific functions.

2. Attrition by Overtime Status
sql
SELECT
    OverTime,
    COUNT(*) AS TotalEmployees,
    SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS EmployeesLeft,
    ROUND(100.0 * SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 1) AS AttritionRatePercent
FROM ibm_hr
WHERE OverTime != 'OverTime'
GROUP BY OverTime
ORDER BY AttritionRatePercent DESC;

Finding: Employees who work overtime leave at nearly three times the rate of those who do not, 30.5% versus 10.4%. This is the strongest single relationship found in the dataset and points to overtime as a leading indicator of attrition risk.

3. Average Monthly Income by Department and Job Role
sql
SELECT
    Department,
    JobRole,
    COUNT(*) AS TotalEmployees,
    ROUND(AVG(MonthlyIncome), 0) AS AvgMonthlyIncome
FROM ibm_hr
WHERE Department != 'Department'
GROUP BY Department, JobRole
ORDER BY Department, AvgMonthlyIncome DESC;

Finding: Pay varies steeply within each department based on role seniority. In Research & Development, for example, Managers average 17,130 while Laboratory Technicians average 3,237, roughly a five times difference within the same department. This income gradient is worth examining alongside attrition patterns in future analysis.

4. Attrition by Job Satisfaction Level
sql
SELECT
    JobSatisfaction,
    COUNT(*) AS TotalEmployees,
    SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) AS EmployeesLeft,
    ROUND(100.0 * SUM(CASE WHEN Attrition = 'Yes' THEN 1 ELSE 0 END) / COUNT(*), 1) AS AttritionRatePercent
FROM ibm_hr
WHERE JobSatisfaction != 'JobSatisfaction'
GROUP BY JobSatisfaction
ORDER BY JobSatisfaction;

Finding: Attrition decreases as job satisfaction increases, from 22.8% at the lowest satisfaction level (1) down to 11.3% at the highest (4). Employees with the lowest satisfaction scores are roughly twice as likely to leave as those with the highest.

5. Average Tenure: Employees Who Left vs Stayed
sql
SELECT
    Attrition,
    COUNT(*) AS TotalEmployees,
    ROUND(AVG(YearsAtCompany), 1) AS AvgYearsAtCompany
FROM ibm_hr
WHERE Attrition != 'Attrition'
GROUP BY Attrition;

Finding: Employees who left the company averaged 5.1 years of tenure, compared to 7.4 years for those who stayed. Employees who leave tend to do so somewhat earlier in their time at the company, though the gap is moderate rather than a sharp early cliff.

Summary

This analysis combined filtering, aggregation, multi-level grouping and conditional logic (CASE statements) to move from raw employee records to five practical findings on attrition. The clearest and most actionable signal is overtime, employees working overtime leave at nearly three times the rate of those who do not, alongside a genuine link between low job satisfaction and higher attrition. Attrition is also concentrated in Sales and HR rather than spread evenly, and employees who leave tend to do so a few years earlier in their tenure than those who stay. Together these findings point toward concrete areas an HR team could investigate further, workload and overtime policy in high attrition departments, and targeted retention efforts for employees showing low job satisfaction.
Related

Business analysis case study based on this project
Related

Business analysis case study based on this project
