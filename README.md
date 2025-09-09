# 📦 Reducing Return-Driven Revenue Loss: E-Commerce Analysis with Olist Dataset  
Analysis of e-commerce returns using the Olist Brazilian dataset (Python + Colab + Tableau)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dustinsherratt/ecommerce-returns-analysis/blob/main/notebooks/ecommerce_return_analysis.ipynb)

## 📊 Project Overview  
This project explores **customer return behavior** in e-commerce using the **Olist Brazilian E-commerce Dataset**.  
The goal is to identify return patterns, quantify revenue loss, and provide insights that can help online retailers reduce avoidable returns.  

The analysis was performed in **Python (Google Colab)** with **pandas, matplotlib, and seaborn**, and results were summarized in both a **Tableau dashboard** and a **one-page datafolio**.  

---

## 📂 Dataset  
- **Source:** [Olist E-commerce Dataset (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)  
- **Tables Used:**  
  - `orders` – Order lifecycle  
  - `order_items` – Products per order  
  - `order_payments` – Payment details  
  - `order_reviews` – Customer satisfaction  
  - `customers` – Demographics  
  - `sellers` – Seller details  
  - `geolocation` – City & state mapping  
  - `products` – Product categories & attributes  
  - `product_category_name_translation` – English translations  

---

## 🛠️ Methods  
1. **Data Cleaning & Preparation**  
   - Removed duplicates and nulls  
   - Standardized timestamps  
   - Merged datasets for unified analysis  

2. **Exploratory Data Analysis (EDA)**  
   - Return rates by product category  
   - Customer review scores vs. return likelihood  
   - Geographical hotspots of returns  
   - Revenue loss estimation  

3. **Visualization & Reporting**  
   - Python plots for trends  
   - Tableau dashboard for interactive insights  
   - One-page datafolio for quick summary  

---

## 🔑 Key Insights  
- **Electronics & apparel** have the highest return rates.  
- **Low review scores (1–2 stars)** strongly correlate with returns.  
- Returns create **significant revenue loss** across several states.  
- **São Paulo & Rio de Janeiro** are top regions for returns.  

---

## 📈 Results & Deliverables  
- 📊 **Interactive Tableau Dashboard**: [View Here](https://public.tableau.com/app/profile/dustin.sherratt/viz/Portfolio_17519795010140/Dashboard1)
- 📄 **One-Page Datafolio (PDF)**: (to be uploaded in repo)  
- 📑 **Data Curation Summaries**: see `/docs/`  

---

## 🚀 How to Run This Project  
### Option 1: Run in Google Colab (Recommended)  
Click the badge at the top of this page to open the notebook directly in Colab.  

### Option 2: Run Locally  
1. Clone the repo:  
   ```bash
   git clone https://github.com/dustinsherratt/ecommerce-returns-analysis.git
   cd ecommerce-returns-analysis

2. Install requirements:
   ```bash
   pip install -r requirements.txt

3. Open the notebook in Jupyter:
   ```bash
   jupyter notebook notebooks/ecommerce_return_analysis.ipynb
