# 👥 Customer360_SQL Project

Welcome to the **Customer360_SQL Project**!

This project showcases how to clean, transform, and analyze customer behavior data using SQL. It includes practical data preparation steps such as renaming columns, removing nulls and duplicates, and uncovering insights about customer income, age, gender, and spending patterns.

## 📂 Dataset

**Main Table Used:** `data.customers`

### View All Data
```sql
SELECT * FROM data.customers;
```

## 🧹 Data Cleaning Tasks
```sql
ALTER TABLE data.customers
DROP COLUMN MyUnknownColumn_[0],
DROP COLUMN MyUnknownColumn_[1],
DROP COLUMN MyUnknownColumn_[2],
DROP COLUMN MyUnknownColumn_[3],
DROP COLUMN MyUnknownColumn_[4],
DROP COLUMN MyUnknownColumn_[5],
DROP COLUMN MyUnknownColumn_[6];

ALTER TABLE data.customers 
RENAME COLUMN `Annual Income ($)` TO Annual_Income;

ALTER TABLE data.customers 
RENAME COLUMN `Spending Score (1-100)` TO Spending_Score;

DELETE FROM data.customers WHERE Profession IS NULL;
DELETE FROM data.customers WHERE Age IS NULL OR Gender IS NULL OR Annual_Income IS NULL;

DELETE t1 FROM data.customers t1
JOIN data.customers t2 
ON t1.CustomerID > t2.CustomerID 
AND t1.Gender = t2.Gender 
AND t1.Age = t2.Age 
AND t1.Annual_Income = t2.Annual_Income 
AND t1.Spending_Score = t2.Spending_Score;
```

## 📊 Insightful SQL Analysis
```sql
SELECT CustomerID, Annual_Income, Spending_Score,
       (Annual_Income * Spending_Score) AS customer_value_index
FROM data.customers
ORDER BY customer_value_index DESC
LIMIT 5;

SELECT 
  CASE 
    WHEN Age BETWEEN 0 AND 16 THEN '0-16'
    WHEN Age BETWEEN 17 AND 25 THEN '17-25'
    WHEN Age BETWEEN 26 AND 35 THEN '26-35'
    WHEN Age BETWEEN 36 AND 45 THEN '36-45'
    WHEN Age BETWEEN 46 AND 55 THEN '46-55'
    WHEN Age BETWEEN 56 AND 65 THEN '56-65'
    WHEN Age BETWEEN 66 AND 75 THEN '66-75'
    WHEN Age BETWEEN 76 AND 85 THEN '76-85'
    ELSE '90+'
  END AS age_tier,
  AVG(Spending_Score) AS avg_spending_score
FROM data.customers
GROUP BY age_tier
ORDER BY age_tier DESC;

SELECT CustomerID, Annual_Income, Spending_Score,
       CASE 
         WHEN Annual_Income > (SELECT AVG(Annual_Income) FROM data.customers)
         AND Spending_Score < (SELECT AVG(Spending_Score) FROM data.customers)
         THEN 'High Income / Low Spending'
         ELSE 'Other'
       END AS customer_type
FROM data.customers;

SELECT Gender, AVG(Spending_Score) AS avg_spending_score 
FROM data.customers 
GROUP BY Gender;

SELECT Gender, Age, Annual_Income, Spending_Score
FROM data.customers
ORDER BY Spending_Score DESC
LIMIT 10;

SELECT CustomerID, Annual_Income, Spending_Score, Age
FROM data.customers
WHERE Age < (SELECT AVG(Age) FROM data.customers)
  AND Spending_Score > (SELECT AVG(Spending_Score) FROM data.customers)
ORDER BY Age;

SELECT CustomerID, Annual_Income, Spending_Score, Age
FROM data.customers
WHERE Age > (SELECT AVG(Age) FROM data.customers)
  AND Spending_Score > (SELECT AVG(Spending_Score) FROM data.customers)
ORDER BY Age DESC
LIMIT 10;

SELECT CustomerID, Annual_Income, Spending_Score
FROM data.customers
ORDER BY Annual_Income DESC;
```

## 👩‍💻 Author

**Rukayat Adebisi Obanor**  
Junior Data Analyst | SQL Enthusiast | Instructor  

📧 Email: bisiobanor@gmail.com  
🔗 LinkedIn: [linkedin.com/in/rukayatobanor](https://www.linkedin.com/in/rukayatobanor)  
📁 Portfolio: [datascienceportfol.io/RukayatAdebisiObanor](https://www.datascienceportfol.io/RukayatAdebisiObanor)

## 📜 License

This project is open for learning, practice, and educational use. Feel free to fork, share, or contribute.
