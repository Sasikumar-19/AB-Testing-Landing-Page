#  Data Storytelling & Statistical Validation  
 

This repository contains the complete workflow for **Task-4**, where I performed **Data Storytelling and Statistical Validation using A/B Testing analysis**.

The objective of this task was to synthesize analytical insights into a **clear business narrative** and validate findings using **statistical hypothesis testing**.

The analysis evaluates whether a **new landing page improves conversion rates compared to the existing page**.

---

# 🔧 1️⃣ Data Loading & Preprocessing (Working Process)

## ✔ Dataset Used
The dataset used in this task represents an **A/B testing experiment** conducted to compare two landing pages.

Columns included:

- user_id  
- timestamp  
- group  
- landing_page  
- converted  

Where:

- **Control group** → Users who saw the old landing page  
- **Treatment group** → Users who saw the new landing page  

---

## ✔ Data Cleaning

The following preprocessing steps were performed:

- Loaded dataset using **Pandas**
- Checked dataset structure and missing values
- Removed duplicate users
- Ensured consistency between group and landing page assignments

These steps ensured that the dataset was **clean and suitable for statistical analysis**.

---

# 📊 2️⃣ Exploratory Data Analysis (EDA)

## ✔ Group Distribution

Analyzed the number of users in both groups to ensure a **balanced experimental setup**.

Groups analyzed:

- Control (Old Page)
- Treatment (New Page)

Balanced groups ensure **valid A/B testing results**.

---

## ✔ Conversion Rate Analysis

Conversion rate represents the percentage of users who completed the desired action.

Example:

| Group | Conversion Rate |
|------|------|
| Control | ~12.02% |
| Treatment | ~11.88% |

Visualizations were created to compare conversion performance across groups.

---

# 🔍 3️⃣ Hypothesis Testing

Statistical hypothesis testing was performed to determine whether the difference in conversion rates is statistically significant.

---

## ⭐ Hypothesis Definition

### Null Hypothesis (H₀)

The new landing page does **not increase conversion rate**.

H₀: Conversion rate (new page) ≤ Conversion rate (old page)

---

### Alternative Hypothesis (H₁)

The new landing page **increases conversion rate**.

H₁: Conversion rate (new page) > Conversion rate (old page)

---

## ⭐ Statistical Test Used

A **Z-Test for proportions** was used to compare conversion rates between the control and treatment groups.

This test determines whether the observed difference occurred due to **random chance or actual improvement**.

---

## ⭐ Test Results

| Metric | Value |
|------|------|
| Z-Statistic | ~1.31 |
| P-Value | ~0.226 |

Decision Rule:

If p-value < 0.05 → Reject Null Hypothesis  
If p-value ≥ 0.05 → Fail to Reject Null Hypothesis

Since:

p-value = 0.189 > 0.05

We **fail to reject the null hypothesis**.

---

# 📈 4️⃣ Data Storytelling & Business Interpretation

The results were translated into a **clear business narrative** for stakeholders.

Key observations:

- Conversion rates between both landing pages are very similar
- The statistical test shows the difference is **not statistically significant**
- Deploying the new landing page would not significantly improve conversion performance

---

# 📑 5️⃣ Business Recommendation

Based on the statistical results:

- The company should **continue using the current landing page**
- Further experiments or design improvements should be tested before replacing the existing page

Using statistical validation ensures **data-driven decision making** rather than assumptions.

---

# 🛠 6️⃣ Tools & Technologies Used

- Python
- Pandas
- NumPy
- Seaborn
- Matplotlib
- Statistical Testing (Z-Test)
- Jupyter Notebook
 

---
 
