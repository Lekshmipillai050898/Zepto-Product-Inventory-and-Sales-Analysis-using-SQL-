# Zepto Product Inventory and Sales Analysis using SQL

## Project Overview

This project uses SQL to explore, clean, and analyze the `zepto_v2` dataset, which contains product-level information from an online retail platform. The goal is to find insights about inventory, discounts, pricing, and potential revenue.

## Dataset Information

The dataset includes:

* **name** → Product name
* **category** → Product category
* **mrp** → Maximum Retail Price
* **discountPercent** → Discount percentage
* **availableQuantity** → Quantity in stock
* **discountedSellingPrice** → Selling price after discount
* **weightInGms** → Product weight in grams
* **outOfStock** → 1 = out of stock, 0 = in stock
* **quantity** → Per-order quantity

##  Objectives

* Understand the data structure
* Clean incorrect or missing data
* Analyze discounts and pricing trends
* Estimate revenue by category
* Analyze inventory (stock vs. out-of-stock)
* Group products by weight for better understanding


## Data Exploration

### Total Rows in the Dataset

SELECT COUNT(*) AS number_of_rows FROM zepto_v2;

###  Sample Data

SELECT TOP 10 * FROM zepto_v2;

### Check for Null Values

SELECT * FROM zepto_v2
WHERE name IS NULL 
OR category IS NULL
OR mrp IS NULL
OR discountPercent IS NULL
OR availableQuantity IS NULL
OR discountedSellingPrice IS NULL
OR weightInGms IS NULL
OR outOfStock IS NULL
OR quantity IS NULL;


### Unique Categories

SELECT DISTINCT category FROM zepto_v2 ORDER BY category;

### Stock Status Distribution

SELECT outOfStock, COUNT(*) FROM zepto_v2 GROUP BY outOfStock;

## Data Cleaning

Removed rows where:

* mrp = 0
* discountedSellingPrice = 0


DELETE FROM zepto_v2 WHERE mrp = 0;


##  Key Analysis & Insights

###  Top 10 Products by Discount Percentage

SELECT TOP 10 name, discountPercent
FROM zepto_v2
ORDER BY discountPercent DESC;
<img width="582" height="302" alt="image" src="https://github.com/user-attachments/assets/7f1e0045-def0-4aed-bd38-cf34dcbcb041" />


**Finding:** These products have the biggest discounts, which may attract customers but could reduce profit margins if not managed carefully.

---

### 📦 High MRP but Out-of-Stock Products

```sql
SELECT DISTINCT name, mrp FROM zepto_v2
WHERE outOfStock = 1 AND mrp > 30000;
```

**Finding:** Some expensive products are out of stock. These might be in high demand or face supply issues.

---

### 💰 Estimated Revenue by Category

```sql
SELECT category,
       SUM(discountedSellingPrice * availableQuantity) AS Total_Revenue
FROM zepto_v2
GROUP BY category;
```

**Finding:** Some categories contribute significantly more to revenue than others. Focusing on high-revenue categories could help grow the business.

---

### 🛍️ Products with Low Discounts & High MRP

```sql
SELECT DISTINCT name, mrp, discountPercent
FROM zepto_v2
WHERE mrp > 50000 AND discountPercent < 10;
```

**Finding:** These are premium products with low discounts, suggesting a strategy of maintaining brand value rather than offering deep discounts.

---

### 🎯 Top 5 Categories with Highest Average Discounts

```sql
SELECT TOP 5 category,
              AVG(discountPercent) AS Average_Discount
FROM zepto_v2
GROUP BY category;
```

**Finding:** These categories are heavily discounted, perhaps because they have surplus inventory or are part of marketing promotions.

---

### ⚖️ Price per Gram Analysis

```sql
SELECT name, weightInGms, discountedSellingPrice,
       discountedSellingPrice / weightInGms AS Price_per_Gram
FROM zepto_v2
WHERE weightInGms > 100
ORDER BY Price_per_Gram DESC;
```

**Finding:** Helps identify which products offer better value for money. Customers may prefer products with a lower price per gram.

---

### 📏 Product Weight Segmentation

```sql
CASE
    WHEN weightInGms < 1000 THEN 'Low'
    WHEN weightInGms < 5000 THEN 'Medium'
    ELSE 'Bulk'
END AS Weight_Category
```

**Finding:** Categorizing products by weight helps with packaging, shipping, and marketing strategies.

---

### 🏋️ Total Inventory Weight per Category

```sql
SELECT category,
       SUM(weightInGms * availableQuantity) AS Total_Weight
FROM zepto_v2
GROUP BY category;
```

**Finding:** This helps understand storage and logistics requirements for each product category.

---

## ✅ Summary of Findings

* Most products are **in stock**, but some high-priced items are **out of stock**, suggesting potential revenue loss if restocking isn’t managed.
* A few categories have very **high discounts**, indicating clearance strategies or marketing efforts.
* **Revenue is concentrated** in certain categories, suggesting where the business might focus for growth.
* Premium products are sold with **minimal discounts**, preserving brand value.
* **Price per gram analysis** highlights products offering better value to customers.
* **Weight categories** help simplify logistics and marketing decisions.

---

## 🔚 Conclusions

* The business is discounting products smartly but should monitor high discounts to avoid profit loss.
* Restocking high-MRP products might help capture more revenue.
* Focus on high-revenue categories can boost profits.
* Price-per-gram analysis can guide better product recommendations for customers.

---

## 💡 Suggestions

✅ **Inventory Planning:**

* Prioritize restocking high-value out-of-stock products.

✅ **Discount Strategies:**

* Review high discounts to ensure they’re sustainable for the business.

✅ **Product Promotion:**

* Promote good-value products (low price per gram) as budget-friendly options.

✅ **Category Focus:**

* Invest more in high-revenue categories for better returns.

✅ **Logistics:**

* Use weight segmentation to optimize packaging and shipping costs.


