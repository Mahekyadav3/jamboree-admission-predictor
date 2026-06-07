# 📋 Detailed Findings & Business Insights

## Project: Graduate Admission Chance Predictor — Jamboree Education

**Author: Mahek Yadav** | [GitHub](https://github.com/Mahekyadav3) · [LinkedIn](https://www.linkedin.com/in/mahek-yadav-b48b13358) · [yadavmahek00@gmail.com](mailto:yadavmahek00@gmail.com)

---

## 1. Data Overview

- **Dataset size:** 500 rows × 8 columns (after dropping `Serial No.`)
- **No missing values** were found
- **No duplicate rows** were found
- **Outliers:** A few outliers exist in some features, but they represent valid applicant profiles (extreme but real academic scores), so no removal was performed

---

## 2. Exploratory Data Analysis

### Univariate Analysis

| Feature | Observation |
|---|---|
| GRE Score | Slightly skewed toward higher scores; most applicants score between 300–340 |
| TOEFL Score | Concentrated in the upper range; most applicants score 100–120 |
| CGPA | Most students fall between 8.0–9.5; distribution is roughly normal |
| SOP & LOR | Mid-to-high range; 3.0–4.5 is the most common band |
| University Rating | Spread across all levels; no extreme concentration |
| Chance of Admit | Fairly continuous and not heavily skewed; suitable for regression |
| Research | Both categories (0 and 1) are well-represented |

### Bivariate Analysis

| Predictor | Relationship with Admit Chance |
|---|---|
| CGPA | Strongest positive correlation |
| GRE Score | Strong positive linear relationship |
| TOEFL Score | Strong positive linear relationship |
| LOR | Moderate positive relationship |
| SOP | Moderate positive relationship |
| University Rating | Moderate positive relationship |
| Research | Students with research experience have higher median admit chance |

**Notable inter-feature correlation:** GRE, TOEFL, and CGPA are also correlated with each other, which introduces multicollinearity risk. This was handled using VIF analysis before finalizing the model.

---

## 3. Model Building

### OLS Linear Regression (Full Model)

- All 7 features were initially included
- Model was fit using `statsmodels.OLS` for interpretability (p-values, confidence intervals)
- Several features showed high VIF, indicating multicollinearity

### Multicollinearity Treatment (VIF)

Features with VIF > 5 were iteratively removed until all remaining features had acceptable VIF (≤ 5). This ensures that coefficient estimates are stable and interpretable.

### Assumption Checks

**1. Mean of Residuals ≈ 0**
Residuals had a mean very close to zero, satisfying this assumption.

**2. Linearity (Residuals vs Fitted)**
The residuals vs fitted plot showed no systematic curve or funnel shape, indicating that the linear assumption is reasonably met.

**3. Homoscedasticity (Breusch-Pagan Test)**
Breusch-Pagan p-value > 0.05 → Fail to reject the null hypothesis → **Constant variance of residuals assumed.**

**4. Normality of Residuals (Shapiro-Wilk + QQ Plot)**
The histogram of residuals was approximately bell-shaped and the QQ plot showed values aligned close to the 45° reference line, indicating residuals are approximately normally distributed.

---

## 4. Regularized Models

### Why Regularization?
Since GRE, TOEFL, and CGPA are correlated, regularization can help stabilize coefficients without requiring manual feature removal.

### Ridge Regression
- Penalizes large coefficients but retains all features
- Performed similarly to OLS on test data
- Useful when all predictors are potentially relevant

### Lasso Regression
- Can shrink some coefficients to exactly zero, performing implicit feature selection
- With alpha=0.001, most coefficients were retained
- Performance comparable to Ridge and OLS

**Conclusion:** The dataset's linear structure means OLS is already a strong model. Ridge and Lasso add stability but don't dramatically change performance.

---

## 5. Key Findings

### Feature Importance (from OLS coefficients)

1. **CGPA** — Highest positive coefficient; the single most predictive feature
2. **GRE Score** — Second most impactful academic factor
3. **TOEFL Score** — Closely related to GRE in importance
4. **Research Experience** — Binary variable with meaningful lift on admit probability
5. **LOR** — Letter quality contributes moderately
6. **SOP** — Statement quality contributes moderately
7. **University Rating** — Partially absorbed by academic scores in the final model

### Statistical Significance

- Most features are statistically significant (p-value < 0.05) in the full model
- After VIF-based feature removal, remaining coefficients are more reliable

---

## 6. Business Recommendations

### For Jamboree's Admission Prediction Tool

| # | Recommendation | Rationale |
|---|---|---|
| 1 | Highlight CGPA, GRE, and TOEFL as the main predictors in UI | These have the strongest statistical relationship with admit chance |
| 2 | Show admit probability as a range (e.g., 65%–75%) not a point estimate | Regression outputs have uncertainty; communicating a range is more honest |
| 3 | Flag students with Research = 0 and suggest research opportunities | Research has a measurable positive effect on admission probability |
| 4 | Build personalized improvement plans based on weak features | A student low on LOR/SOP can be guided to improve those specific areas |
| 5 | Collect richer data to improve the model | Add: internship experience, publications, target program, university admit history |
| 6 | Retrain the model periodically | Admission trends can shift; the model should reflect current data |
| 7 | Do not use predictions as guaranteed outcomes | Present as estimates, not guarantees — maintain legal/ethical responsibility |

---

## 7. Model Limitations

- The model assumes a **linear relationship** between features and admit chance; real admissions may involve non-linear dynamics
- **University-specific preferences** are not captured (some schools prioritize GRE over CGPA)
- The dataset has **500 records** — a larger, more recent dataset would improve reliability
- **Socioeconomic, geographic, and program-specific factors** are not included

---

## 8. Future Scope

- Try **non-linear models** (Random Forest, XGBoost) to capture complex patterns
- Implement **cross-validation** for more robust evaluation
- Build a **Streamlit or Flask web app** for Jamboree's prediction interface
- Extend to **multi-class classification** (e.g., High/Medium/Low admit chance) for easier user interpretation
