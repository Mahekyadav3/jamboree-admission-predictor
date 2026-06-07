# 🎓 Graduate Admission Chance Predictor — Jamboree Education

A machine learning project that predicts a student's **probability of admission** to graduate programs using Linear Regression and regularization techniques (Ridge & Lasso). Built for Jamboree Education to power their online admission estimation feature.

---

## 📌 Problem Statement

Jamboree Education wants to help students estimate their chances of getting into graduate school based on their academic and profile-related factors. This project builds and validates a regression model that can serve as a baseline predictor on their platform.

---

## 📂 Dataset

- **Source:** [Jamboree Admission Dataset](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/001/839/original/Jamboree_Admission.csv)
- **Records:** 500 applicant profiles
- **Target Variable:** `Chance of Admit` (continuous, 0 to 1)

### Features

| Feature | Description |
|---|---|
| GRE Score | Graduate Record Exam score (out of 340) |
| TOEFL Score | English proficiency score (out of 120) |
| University Rating | Prestige of the target university (1–5) |
| SOP | Statement of Purpose strength (1–5) |
| LOR | Letter of Recommendation strength (1–5) |
| CGPA | Undergraduate GPA (out of 10) |
| Research | Research experience (0 = No, 1 = Yes) |

---

## 🔍 Project Workflow

```
Data Loading & Cleaning
        ↓
Exploratory Data Analysis (EDA)
        ↓
Preprocessing (Outlier check, Feature encoding)
        ↓
Model Building (OLS Linear Regression)
        ↓
Assumption Validation (VIF, Breusch-Pagan, Shapiro-Wilk)
        ↓
Model Evaluation (MAE, RMSE, R², Adjusted R²)
        ↓
Regularization (Ridge & Lasso)
        ↓
Key Findings & Recommendations
```

---

## 📊 Exploratory Data Analysis

- **Univariate:** Distribution plots and boxplots for all continuous features
- **Bivariate:** Correlation heatmap, scatter plots against target, boxplot for Research vs Admit Chance
- **Key EDA Findings:**
  - CGPA shows the strongest positive correlation with admission chances
  - Students with research experience tend to have higher median admit probability
  - GRE, TOEFL, and CGPA are moderately correlated with each other — multicollinearity check is necessary

---

## 🤖 Modeling

### 1. OLS Linear Regression (statsmodels)
- Full model fit, coefficient significance check via p-values
- VIF-based iterative multicollinearity removal (threshold: VIF > 5)
- Refitted model after dropping high-VIF features

### 2. Assumption Validation
| Assumption | Test Used | Outcome |
|---|---|---|
| Multicollinearity | Variance Inflation Factor (VIF) | Features with VIF > 5 dropped |
| Mean of Residuals | Manual check | ≈ 0 ✅ |
| Linearity | Residuals vs Fitted plot | No visible pattern ✅ |
| Homoscedasticity | Breusch-Pagan test | p-value > 0.05 ✅ |
| Normality of Residuals | Shapiro-Wilk + QQ Plot | Approximately normal ✅ |

### 3. Regularized Models (sklearn)
- **Ridge Regression** (alpha=1.0)
- **Lasso Regression** (alpha=0.001)
- Features standardized using `StandardScaler` before regularization

---

## 📈 Results

| Model | Train R² | Test R² | Test RMSE |
|---|---|---|---|
| OLS (after VIF) | ~0.82 | ~0.80 | Low |
| Ridge | ~0.82 | ~0.81 | Low |
| Lasso | ~0.82 | ~0.81 | Low |

> All three models perform comparably, indicating the data fits a linear relationship well and there is no severe overfitting.

---

## 💡 Key Findings

- **CGPA** is the most influential predictor of admission probability
- **GRE Score** and **TOEFL Score** are the next most important academic signals
- **Research experience** meaningfully improves admission chances
- **SOP, LOR, and University Rating** have moderate contributions

---

## ✅ Recommendations for Jamboree

1. Emphasize **CGPA, GRE, and TOEFL** as the primary inputs in the admission probability tool
2. Provide targeted improvement plans for students with low predicted scores
3. Display predictions as **probability ranges** rather than exact values to communicate uncertainty responsibly
4. Enrich the model with additional features: work experience, research publications, undergraduate institution, intended program, and university-specific admit history
5. Do not present the model as a guaranteed decision — use it as a directional estimator

---

## 🛠️ Tech Stack

| Library | Purpose |
|---|---|
| `pandas` | Data manipulation |
| `numpy` | Numerical operations |
| `matplotlib` / `seaborn` | Visualization |
| `statsmodels` | OLS regression, VIF, diagnostic tests |
| `scikit-learn` | Ridge, Lasso, train-test split, metrics |
| `scipy` | Shapiro-Wilk normality test |

---

## 🚀 Getting Started

### Prerequisites
```bash
pip install numpy pandas matplotlib seaborn scikit-learn statsmodels scipy
```

### Run the Notebook
```bash
git clone https://github.com/YOUR_USERNAME/jamboree-admission-predictor.git
cd jamboree-admission-predictor
jupyter notebook LinearRegJamboreeAdmission.ipynb
```

---

## 📁 Repository Structure

```
jamboree-admission-predictor/
│
├── LinearRegJamboreeAdmission.ipynb   # Main analysis notebook
├── README.md                          # Project documentation
├── FINDINGS.md                        # Detailed insights and recommendations
└── requirements.txt                   # Python dependencies
```

---

## 📄 License

This project is for educational purposes as part of a data science case study.

---

## 🙋 Author

**Mahek Yadav**

[![GitHub](https://img.shields.io/badge/GitHub-Mahekyadav3-181717?style=flat&logo=github)](https://github.com/Mahekyadav3)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-mahek--yadav-0077B5?style=flat&logo=linkedin)](https://www.linkedin.com/in/mahek-yadav-b48b13358)
[![Email](https://img.shields.io/badge/Email-yadavmahek00@gmail.com-D14836?style=flat&logo=gmail)](mailto:yadavmahek00@gmail.com)
