Bank Loan Data Analysis using SQL

**Project Overview**
This project focuses on analyzing bank loan data using SQL to extract meaningful insights related to loan distribution, repayment behavior, and customer segmentation.

The goal of this project is to demonstrate how SQL can be used to analyze structured financial data and support decision-making.

**Objectives**

- Analyze total loan applications and funded amounts  
- Understand loan status distribution  
- Identify high-risk loan segments  
- Analyze customer verification behavior  
- Study monthly loan trends

- This project also includes advanced SQL concepts to perform deeper analysis:

- Window Functions (RANK, ROW_NUMBER, DENSE_RANK)
- Partitioning using PARTITION BY
- Running Totals and Moving Averages
- Percentage calculations using window functions

**Dataset Description**

The dataset contains information about:

- Loan Amount  
- Total Payment  
- Loan Status  
- Interest Rate  
- Grade and Sub-grade  
- Issue Date  
- Verification Status  
- Home Ownership  

**Key SQL Queries**

**Total Loan Applications**

-- 1. Total Loan Applications
SELECT COUNT(*) AS total_applications
FROM loan_data;

-- 2. Total Funded Amount
SELECT SUM(loan_amount) AS total_funded_amount
FROM loan_data;

-- 3. Total Amount Received
SELECT SUM(total_payment) AS total_received_amount
FROM loan_data;

-- 4. Loan Status Distribution
SELECT loan_status, COUNT(*) AS total_loans
FROM loan_data
GROUP BY loan_status
ORDER BY total_loans DESC;

-- 5. Average Interest Rate by Grade
SELECT grade, ROUND(AVG(int_rate),2) AS avg_interest_rate
FROM loan_data
GROUP BY grade
ORDER BY grade;

-- 6. Monthly Loan Applications
SELECT MONTH(issue_date) AS month, COUNT(*) AS total_loans
FROM loan_data
GROUP BY MONTH(issue_date)
ORDER BY month;

-- 7. Verified vs Non-Verified Customers
SELECT verification_status, COUNT(*) AS total
FROM loan_data
GROUP BY verification_status;

-- 8. Home Ownership Analysis
SELECT home_ownership, COUNT(*) AS total_loans
FROM loan_data
GROUP BY home_ownership;

-- 9. High Risk Loans (Charged Off)
SELECT grade, COUNT(*) AS risky_loans
FROM loan_data
WHERE loan_status = 'Charged Off'
GROUP BY grade
ORDER BY risky_loans DESC;

-- 10. Top 5 Highest Loan Amounts
SELECT *
FROM loan_data
ORDER BY loan_amount DESC
LIMIT 5;

--11. Rank Loans by Amount (Window Function)
SELECT 
    id,
    loan_amount,
    RANK() OVER (ORDER BY loan_amount DESC) AS loan_rank
FROM loan_data;

--12. Top 3 Loans in Each Grade
SELECT *
FROM (
    SELECT 
        grade,
        loan_amount,
        ROW_NUMBER() OVER (PARTITION BY grade ORDER BY loan_amount DESC) AS rn
    FROM loan_data
) t
WHERE rn <= 3;

--13. Running Total of Loans
SELECT 
    issue_date,
    loan_amount,
    SUM(loan_amount) OVER (ORDER BY issue_date) AS running_total
FROM loan_data;

--14. Moving Average (Trend Analysis)
SELECT 
    issue_date,
    loan_amount,
    AVG(loan_amount) OVER (
        ORDER BY issue_date 
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_avg
FROM loan_data;

--15. Percentage Contribution by Loan Status
SELECT 
    loan_status,
    COUNT(*) * 100.0 / SUM(COUNT(*)) OVER () AS percentage
FROM loan_data
GROUP BY loan_status;

--16. Find Duplicate Customers
SELECT customer_id, COUNT(*) AS cnt
FROM loan_data
GROUP BY customer_id
HAVING COUNT(*) > 1;

--17. Highest Loan per Customer
SELECT *
FROM (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY loan_amount DESC) AS rn
    FROM loan_data
) t
WHERE rn = 1;

--18. 

