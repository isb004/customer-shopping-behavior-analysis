# Customer Shopping Behavior Analysis & Revenue Optimization

An end-to-end data analytics project evaluating 3,900 transactional records across customer demographics, spending behaviors, and purchase parameters. The project automates data preprocessing in Python, executes structured relational business queries in MySQL, and visualizes executive KPIs through an interactive Power BI dashboard to guide customer retention and revenue growth.

---

## 📌 Business Overview & Problem Statement

Retail organizations frequently struggle with margin loss resulting from untargeted promotions and low subscription conversions among established customers.

The primary business objective is to analyze transaction patterns to solve:

> *"How can the company leverage consumer shopping data to identify trends, improve customer engagement, and optimize marketing and product strategies?"*
> 

This study evaluates customer segmentation, discount elasticity across product lines, shipping tier margins, and demographic revenue concentrations to support data-backed merchandising and marketing operations.

---

## 🏗️ Architecture & Data Workflow

```text
[ Raw CSV: 3,900 Rows ] 
        │
        ▼
[ Python (Pandas / NumPy) ] ──▶ Handling missing data, snake_case normalization, feature engineering
        │
        ▼
[ SQLAlchemy / PyMySQL ] ────▶ Automated MySQL schema creation & batch ingestion
        │
        ▼
[ MySQL 8.0 Database ] ──────▶ 10 relational business queries, CTEs, and window functions
        │
        ▼
[ Power BI Desktop ] ────────▶ Executive KPI cards, cross-filtering slicers, revenue breakdowns

```

---

## 🔬 Data Processing & Engineering (Python)

* **Dataset Scale:** 3,900 rows across 18 consumer attributes.


* **Preserving Category Variance:** Imputed 37 missing values in `Review Rating` using the category-specific median rather than a global mean, avoiding skewed category sentiment benchmarks.


* **Feature Engineering:**
* Binned customer ages into four life cohorts (`Young Adult`, `Adult`, `Middle-aged`, `Senior`) using quantile distributions.


* Converted textual purchase frequencies (e.g., `Fortnightly`, `Annually`, `Every 3 Months`) into quantitative intervals (`purchase_frequency_days`).




* **Redundancy Auditing:** Evaluated promotional attributes and dropped `promo_code_used` after confirming exact collinearity (`(df['discount_applied'] == df['promo_code_used']).all() == True`).


* **Database Integration:** Programmatically created the database `customer_db` and pushed normalized data directly to the MySQL `customer` table using SQLAlchemy chunking.



---

## 📊 Business Analysis & SQL Findings (MySQL)

### 1. Revenue by Gender
<table>
  <thead>
    <tr>
      <th>Gender</th>
      <th>Total Revenue (USD)</th>
      <th>Revenue Share</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>Male</b></td>
      <td>$157,890</td>
      <td>67.7%</td>
    </tr>
    <tr>
      <td><b>Female</b></td>
      <td>$75,191</td>
      <td>32.3%</td>
    </tr>
  </tbody>
</table>

---

### 2. High-Spending Discount Users
<table>
  <thead>
    <tr>
      <th>Metric</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>Benchmark Average Purchase Amount</b></td>
      <td>$59.76</td>
    </tr>
    <tr>
      <td><b>Qualifying Customers (Discount Used & Spend > Benchmark)</b></td>
      <td>839 customers</td>
    </tr>
    <tr>
      <td><b>Core Finding</b></td>
      <td>Discounts successfully incentivize large basket sizes rather than solely low-value sales.</td>
    </tr>
  </tbody>
</table>

---

### 3. Top 5 Products by Average Rating
<table>
  <thead>
    <tr>
      <th>Rank</th>
      <th>Item Purchased</th>
      <th>Average Product Rating</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>1</td><td>Gloves</td><td>3.86</td></tr>
    <tr><td>2</td><td>Sandals</td><td>3.84</td></tr>
    <tr><td>3</td><td>Boots</td><td>3.82</td></tr>
    <tr><td>4</td><td>Hat</td><td>3.80</td></tr>
    <tr><td>5</td><td>Skirt</td><td>3.78</td></tr>
  </tbody>
</table>

---

### 4. Shipping Type Comparison
<table>
  <thead>
    <tr>
      <th>Shipping Type</th>
      <th>Average Purchase Amount (USD)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>Express</b></td>
      <td>$60.48</td>
    </tr>
    <tr>
      <td><b>Standard</b></td>
      <td>$58.46</td>
    </tr>
  </tbody>
</table>

---

### 5. Subscribers vs. Non-Subscribers Spend Analysis
<table>
  <thead>
    <tr>
      <th>Subscription Status</th>
      <th>Total Customers</th>
      <th>Average Spend (USD)</th>
      <th>Total Revenue (USD)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>Yes (Subscribers)</b></td>
      <td>1,053</td>
      <td>$59.49</td>
      <td>$62,645.00</td>
    </tr>
    <tr>
      <td><b>No (Non-Subscribers)</b></td>
      <td>2,847</td>
      <td>$59.87</td>
      <td>$170,436.00</td>
    </tr>
  </tbody>
</table>

---

### 6. Top 5 Discount-Dependent Products
<table>
  <thead>
    <tr>
      <th>Rank</th>
      <th>Item Purchased</th>
      <th>Discount Rate (%)</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>1</td><td>Hat</td><td>50.00%</td></tr>
    <tr><td>2</td><td>Sneakers</td><td>49.66%</td></tr>
    <tr><td>3</td><td>Coat</td><td>49.07%</td></tr>
    <tr><td>4</td><td>Sweater</td><td>48.17%</td></tr>
    <tr><td>5</td><td>Pants</td><td>47.37%</td></tr>
  </tbody>
</table>

---

### 7. Customer Segmentation by Purchase History
<table>
  <thead>
    <tr>
      <th>Segment</th>
      <th>Criteria (Previous Purchases)</th>
      <th>Number of Customers</th>
      <th>Share of Base</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>Loyal</b></td>
      <td>&gt; 20 orders</td>
      <td>3,116</td>
      <td>79.9%</td>
    </tr>
    <tr>
      <td><b>Returning</b></td>
      <td>5 – 20 orders</td>
      <td>701</td>
      <td>18.0%</td>
    </tr>
    <tr>
      <td><b>New</b></td>
      <td>&lt; 5 orders</td>
      <td>83</td>
      <td>2.1%</td>
    </tr>
  </tbody>
</table>

---

### 8. Top 3 Products per Category
<table>
  <thead>
    <tr>
      <th>Category</th>
      <th>Item Rank</th>
      <th>Item Purchased</th>
      <th>Total Orders</th>
    </tr>
  </thead>
  <tbody>
    <tr><td><b>Accessories</b></td><td>1</td><td>Jewelry</td><td>171</td></tr>
    <tr><td><b>Accessories</b></td><td>2</td><td>Sunglasses</td><td>161</td></tr>
    <tr><td><b>Accessories</b></td><td>3</td><td>Belt</td><td>161</td></tr>
    <tr><td><b>Clothing</b></td><td>1</td><td>Blouse</td><td>171</td></tr>
    <tr><td><b>Clothing</b></td><td>2</td><td>Pants</td><td>171</td></tr>
    <tr><td><b>Clothing</b></td><td>3</td><td>Shirt</td><td>169</td></tr>
    <tr><td><b>Footwear</b></td><td>1</td><td>Sandals</td><td>160</td></tr>
    <tr><td><b>Footwear</b></td><td>2</td><td>Shoes</td><td>150</td></tr>
    <tr><td><b>Footwear</b></td><td>3</td><td>Sneakers</td><td>145</td></tr>
    <tr><td><b>Outerwear</b></td><td>1</td><td>Jacket</td><td>163</td></tr>
    <tr><td><b>Outerwear</b></td><td>2</td><td>Coat</td><td>161</td></tr>
  </tbody>
</table>

---

### 9. Repeat Buyers (>5 Purchases) & Subscription Propensity
<table>
  <thead>
    <tr>
      <th>Subscription Status</th>
      <th>Repeat Buyers Count</th>
      <th>Percentage of Repeat Base</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>Non-Subscribers (No)</b></td>
      <td>2,518</td>
      <td>72.4%</td>
    </tr>
    <tr>
      <td><b>Subscribers (Yes)</b></td>
      <td>958</td>
      <td>27.6%</td>
    </tr>
  </tbody>
</table>

---

### 10. Revenue Contribution by Age Group
<table>
  <thead>
    <tr>
      <th>Age Group</th>
      <th>Total Revenue (USD)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>Young Adult</b></td>
      <td>$62,143</td>
    </tr>
    <tr>
      <td><b>Middle-aged</b></td>
      <td>$59,197</td>
    </tr>
    <tr>
      <td><b>Adult</b></td>
      <td>$55,978</td>
    </tr>
    <tr>
      <td><b>Senior</b></td>
      <td>$55,763</td>
    </tr>
  </tbody>
</table>

---

## 📈 Power BI Executive Dashboard

The interactive Power BI report provides real-time cross-filtering capabilities across demographics and shipping options:

* **Headline KPIs:** Total Customer Count (**3,900**), Average Purchase Amount (**$59.76**), Average Review Rating (**3.75**).


* **Subscription Breakdown:** Visualizes the current **27% subscriber vs. 73% non-subscriber** adoption gap.


* **Sales & Revenue by Category:** Analyzes department performance showing **Clothing** and **Accessories** commanding highest volume and revenue.


* **Demographic Cohort Visuals:** Tracks spend contributions across all binned age brackets.



---

## 💡 Strategic Business Recommendations

1. **Targeted Subscription Conversion:**
* **Opportunity:** 72.4% of proven repeat buyers (>5 purchases) are not subscribed (2,518 customers).


* **Action:** Deploy post-checkout popups offering instant expedited shipping perks or loyalty credits in exchange for annual subscription enrollment.




2. **Mitigate Discount Reliance:**
* **Opportunity:** Top product lines (Hats, Sneakers, Coats) see ~50% promotional dependency.


* **Action:** Transition from flat discounts to threshold-based incentives (e.g., "$15 off purchases over $80") to protect margins while maintaining basket conversions.




3. **Demographic Reallocation:**
* **Opportunity:** Young Adult and Middle-aged groups generate a combined $121,340 in gross revenue.


* **Action:** Prioritize digital and paid social ad spend on high-converting items toward 18–44 demographic cohorts.




4. **Shipping Tier Incentives:**
* **Opportunity:** Express shipping purchasers hold higher average order values ($60.48 vs $58.46).


* **Action:** Offer subsidized express shipping as a tier-2 reward to encourage higher basket sizes.





---

## 📂 Repository Organization

```text
├── data/
│   └── customer_shopping_behavior.csv             # Raw transactional dataset
├── notebooks/
│   └── Customer_Shopping_Behaviour_Analysis.ipynb # Cleaning, feature engineering & MySQL ETL
├── sql/
│   └── mysql_business_queries.sql                 # Production MySQL queries and window logic
├── reports/
│   └── Customer_Shopping_Behavior_Analysis.pdf    # Executive project report
├── .gitignore
├── requirements.txt
└── README.md

```

---

## ⚙️ Setup & Reproducibility

1. **Clone the repository:**
```bash
git clone https://github.com/isb004/customer-shopping-behavior-analysis.git
cd customer-shopping-behavior-analysis

```


2. **Install dependencies:**
```bash
pip install -r requirements.txt

```


3. **Database Ingestion & Analysis:**
* Run `Customer_Shopping_Behaviour_Analysis.ipynb` to clean data and generate the MySQL schema.


* Open `sql/mysql_business_queries.sql` in MySQL Workbench or VS Code to run queries against `customer_db`.


* Open the Power BI dashboard file to view the interactive visualizations.
