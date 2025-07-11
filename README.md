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

### High MRP but Out-of-Stock Products

SELECT DISTINCT name, mrp FROM zepto_v2
WHERE outOfStock = 1 AND mrp > 30000;
<img width="706" height="148" alt="image" src="https://github.com/user-attachments/assets/ad3e701a-2436-4a46-b139-bc6dbcfd6763" />

**Finding:** Some expensive products are out of stock. These might be in high demand or face supply issues.

### Estimated Revenue by Category


SELECT category,
       SUM(discountedSellingPrice * availableQuantity) AS Total_Revenue
FROM zepto_v2
GROUP BY category;
<img width="371" height="399" alt="image" src="https://github.com/user-attachments/assets/0abca7ba-515b-4a2b-8b7e-2cd2105f3236" />


**Finding:** Some categories contribute significantly more to revenue than others. Focusing on high-revenue categories could help grow the business.

### Products with Low Discounts & High MRP

SELECT DISTINCT name, mrp, discountPercent
FROM zepto_v2
WHERE mrp > 50000 AND discountPercent < 10;
<img width="713" height="523" alt="image" src="https://github.com/user-attachments/assets/7d4f956c-b158-424b-b61e-2fe3310cd18b" />


**Finding:** These are premium products with low discounts, suggesting a strategy of maintaining brand value rather than offering deep discounts.


### Top 5 Categories with Highest Average Discounts

SELECT TOP 5 category,
AVG(discountPercent) AS Average_Discount
FROM zepto_v2
GROUP BY category;
<img width="403" height="167" alt="image" src="https://github.com/user-attachments/assets/ca371e7b-fc74-4ed3-9a01-d99932f94058" />

**Finding:** These categories are heavily discounted, perhaps because they have surplus inventory or are part of marketing promotions.

### Price per Gram Analysis

SELECT name, weightInGms, discountedSellingPrice,
       discountedSellingPrice / weightInGms AS Price_per_Gram
FROM zepto_v2
WHERE weightInGms > 100
ORDER BY Price_per_Gram DESC;
<img width="948" height="578" alt="image" src="https://github.com/user-attachments/assets/b775843a-7ade-4187-a521-1983c3cfb770" />

**Finding:** Helps identify which products offer better value for money. Customers may prefer products with a lower price per gram.

### Product Weight Segmentation

select distinct name,weightInGms,
case when weightInGms<1000 then 'Low'
     when weightInGms<5000 then 'Medium'
	 else 'Bulk'
	 end as Weight_Category
from zepto_v2;

<img width="748" height="488" alt="image" src="https://github.com/user-attachments/assets/dc190bbf-7b38-4ffb-9582-7f25966e2395" />

**Finding:** Categorizing products by weight helps with packaging, shipping, and marketing strategies.


###  Total Inventory Weight per Category

SELECT category,
       SUM(weightInGms * availableQuantity) AS Total_Weight
FROM zepto_v2
GROUP BY category;

<img width="362" height="399" alt="image" src="https://github.com/user-attachments/assets/31529caa-4c00-45f8-a091-47c01a6ad94f" />

## Final Findings

High-MRP Products Out of Stock:

Products like Patanjali Cow’s Ghee (₹56,500) and MamyPoko Pants Diapers (₹39,900) are out of stock despite their high price. This suggests either strong demand or supply chain gaps.

Top Revenue Categories:

Munchies and Cooking Essentials each contribute over ₹33,73,690.00 in estimated revenue.

Other strong categories include Personal Care and Paan Corner, each around ₹27,08,490.00.

Fruits & Vegetables generate the lowest revenue (₹10,84,600.00).

## Products with High MRP but Low Discounts

Premium products like Dhara Kachi Ghani Mustard Oil Jar (₹1,25,000) and Saffola Gold Jar (₹1,24,000) offer zero or very low discounts (0-8%).

This shows a strategy to maintain high perceived value rather than attract buyers with discounts.

## Categories with Highest Average Discounts

Fruits & Vegetables offer the highest average discount of 15.46%, possibly to drive quick turnover.

Meats, Fish & Eggs follow with 11.03% average discount.

## Price per Gram

L’Oreal hair color products have the highest price per gram (₹360.47), highlighting a premium product segment.

Products like Praakrit Desi Gir Cow A2 Ghee are significantly cheaper per gram (₹241.67), suggesting better value.

## Weight Segmentation

Most products fall into the Low weight category (<1 kg).

Medium and Bulk products are fewer but significant for logistics planning.

## Total Inventory Weight

Munchies and Cooking Essentials have the highest total inventory weight (~14,04,654 grams), suggesting bulk storage needs.

Meats, Fish & Eggs have the lowest total weight (~48,016 grams), likely due to perishability.

## Conclusions

Certain high-value products are consistently out of stock, signaling high demand or poor inventory management.

Revenue is concentrated in specific categories, highlighting where business focus might yield the best results.

Many premium products maintain low discounts, suggesting a deliberate pricing strategy.

Discount patterns vary across categories, possibly driven by perishability or marketing tactics.

Price per gram analysis reveals premium products vs. value-for-money products, useful for customer targeting.

Weight-based segmentation and inventory weight figures can help in optimizing warehouse space and logistics.

## Suggestions

 Restocking Strategy:

Prioritize replenishing high-MRP products that are out of stock to avoid lost sales.

 Discount Review:

Review high discounts in categories like Fruits & Vegetables to ensure they don’t erode profits unnecessarily.

 Focus on High-Revenue Categories:

Invest marketing and operations efforts into top revenue categories like Munchies, Cooking Essentials, and Personal Care.

 Promote Value Products:

Highlight products offering better price per gram to attract budget-sensitive customers.

 Optimize Logistics:

Use weight segmentation to plan packaging, storage, and transportation more efficiently.

 Demand Forecasting:

Monitor stock levels for high-MRP items to prevent future stockouts.
