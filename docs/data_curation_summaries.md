# Data Curation Summaries
**Project:** Reducing Return-Driven Revenue Loss: Analysis of E-Commerce Customer Behavior  
**Author:** Dustin Sherratt

This document provides a detailed overview of the datasets used in the project, including profiling, cleaning, and schema information.

---

## 1. olist_orders_dataset.csv
**Description:** Orders from 2016 to 2018 made at multiple marketplaces in Brazil at the Olist store.  
**Rows / Columns:** 95,697 × 8  
**Source:** Kaggle - olist_orders_dataset.csv  
**File Size:** 17.65 MB  

### Data Profiling
- Inspected first few rows with `.head()`; checked shape with `.shape`  
- Used `.info()` and `.isnull().sum()` to check data types and missing values  
- Timestamp columns were initially object (string) type  
- Missing values:  
  - `order_approved_at`: 610 nulls  
  - `order_delivered_carrier_date`: 1,783 nulls  
  - `order_delivered_customer_date`: 2,296 nulls  
- Used `.value_counts()` on `order_status` → 8 unique statuses  
- Checked duplicates with `.duplicated().sum()` → none  
- Validated order lifecycle using timestamp comparisons  

### Data Wrangling
- Converted timestamps to `datetime` using `pd.to_datetime()`  
- Created `chronological_valid` column to flag consistent timestamps  
- Created `Keep_in_Analysis` column to retain:  
  1. Delivered and canceled orders with valid chronology  
  2. All unavailable orders  
- Retained only rows flagged `Keep_in_Analysis`  
- Dropped Boolean columns used for cleaning  

### Data Table Schema
| Field | Data Type | Description |
|-------|-----------|------------|
| order_id | object | Unique identifier of the order |
| customer_id | object | Unique customer_id per order |
| order_status | object | Order status (delivered, shipped, etc.) |
| order_purchase_timestamp | datetime64[ns] | Purchase timestamp |
| order_approved_at | datetime64[ns] | Payment approval timestamp |
| order_delivered_carrier_date | datetime64[ns] | Handed to logistic partner |
| order_delivered_customer_date | datetime64[ns] | Actual delivery date |
| order_estimated_delivery_date | datetime64[ns] | Estimated delivery date |

---

## 2. olist_order_items_dataset.csv
**Description:** Data about items purchased within each order  
**Rows / Columns:** 112,650 × 7  
**Source:** Kaggle - olist_order_items_dataset.csv  
**File Size:** 15.44 MB  

### Data Profiling
- Inspected rows and shape; no missing values except `shipping_limit_date` → convert to datetime  
- Key relationships:  
  - 98,666 unique `order_id`s  
  - 3,271 unique products  
  - 3,095 unique sellers  
- Orders with multiple rows indicate multiple items  
- No duplicates  

### Data Wrangling
- Converted `shipping_limit_date` to datetime  
- Verified `price` and `freight_value` are valid  
- Checked item counts per order using `groupby(order_id)`  
- Dataset ready for merging with orders  

### Data Table Schema
| Field | Data Type | Description |
|-------|-----------|------------|
| order_id | object | Unique order identifier |
| order_item_id | int64 | Sequential number of items per order |
| product_id | object | Product identifier |
| seller_id | object | Seller identifier |
| shipping_limit_date | datetime64[ns] | Last date for logistic partner handling |
| price | float64 | Item price |
| freight_value | float64 | Freight cost |

---

## 3. olist_order_payments_dataset.csv
**Description:** Payment information per order  
**Rows / Columns:** 103,886 × 5  
**Source:** Kaggle  
**File Size:** 5.78 MB  

### Data Profiling
- 103,886 rows; 5 columns  
- No missing values; correct data types  
- 99,440 unique `order_id`s → multiple payments per order possible  
- Unique payment types checked with `.value_counts()`  
- No duplicates  

### Data Wrangling
- Cleaned payment methods using `.str.strip().str.lower()`  
- Dropped 'not defined' payments  

### Data Table Schema
| Field | Data Type | Description |
|-------|-----------|------------|
| order_id | object | Unique order identifier |
| payment_sequential | int64 | Payment sequence per order |
| payment_type | object | Payment method |
| payment_installments | int64 | Number of installments |
| payment_value | float64 | Transaction value |

---

## 4. olist_order_reviews_dataset.csv
**Description:** Customer reviews per order  
**Rows / Columns:** 99,224 × 5  
**Source:** Kaggle  
**File Size:** 14.45 MB  

### Data Profiling
- Missing values:  
  - `review_comment_title`: ~58% missing  
  - `review_comment_message`: ~32% missing  
- Each row has unique `review_id`  
- Some orders have multiple reviews  

### Data Wrangling
- Converted review dates to datetime  
- Dropped largely null/redundant columns: `review_comment_title` and `review_answer_timestamp`  

### Data Table Schema
| Field | Data Type | Description |
|-------|-----------|------------|
| review_id | object | Unique review identifier |
| order_id | object | Order identifier |
| review_score | int64 | 1–5 stars |
| review_comment_message | object | Customer comment |
| review_creation_date | datetime64[ns] | Survey sent date |

---

## 5. olist_products_dataset.csv
**Description:** Product details  
**Rows / Columns:** 32,951 × 6  
**Source:** Kaggle  
**File Size:** 2.38 MB  

### Data Profiling
- Checked `.head()`, `.info()`, `.describe()`  
- `product_category_name` missing in ~610 rows  
- Irrelevant columns dropped: name/description lengths, photos, dimensions  

### Data Wrangling
- Filled missing `product_category_name` with 'Unknown'  
- Merged with translation table for English names  

### Data Table Schema
| Field | Data Type | Description |
|-------|-----------|------------|
| product_id | object | Product identifier |
| product_category_name | object | Root category (Portuguese) |
| product_weight_g | float64 | Weight in grams |
| product_length_cm | float64 | Length in cm |
| product_height_cm | float64 | Height in cm |
| product_width_cm | float64 | Width in cm |

---

## 6. olist_customers_dataset.csv
**Description:** Customer information and location  
**Rows / Columns:** 99,441 × 5  
**Source:** Kaggle  
**File Size:** 9.03 MB  

### Data Profiling
- Explored `.head()`, `.info()`, `.describe()`  
- Each row = customer interaction; `customer_unique_id` identifies unique customers  

### Data Wrangling
- Cleaned `customer_city` and `customer_state`  
- Joined with geolocation via `customer_zip_code_prefix`  

### Data Table Schema
| Field | Data Type | Description |
|-------|-----------|------------|
| customer_id | object | Order-specific ID |
| customer_unique_id | object | Unique customer ID |
| customer_zip_code_prefix | object | First 5 digits of zip code |
| customer_city | object | City name |
| customer_state | object | State |

---

## 7. olist_sellers_dataset.csv
**Description:** Sellers fulfilling orders  
**Rows / Columns:** 3,095 × 4  
**Source:** Kaggle  
**File Size:** 174.7 KB  

### Data Profiling
- Verified unique `seller_id`s  
- Cleaned casing and spacing for city/state  

### Data Table Schema
| Field | Data Type | Description |
|-------|-----------|------------|
| seller_id | object | Seller identifier |
| seller_zip_code_prefix | object | First 5 digits of zip code |
| seller_city | object | City name |
| seller_state | object | State |

---

## 8. olist_product_category_name_translation.csv
**Description:** English translation of product categories  
**Rows / Columns:** 71 × 2  
**Source:** Kaggle  
**File Size:** 2.61 kB  

### Profiling & Wrangling
- Converted English names to title format for readability  

### Data Table Schema
| Field | Data Type | Description |
|-------|-----------|------------|
| product_category_name | object | Portuguese name |
| product_category_name_english | object | English translation |

---

## 9. olist_geolocation_dataset.csv
**Description:** Brazilian zip codes with lat/lng  
**Rows / Columns:** 738,332 × 7  
**Source:** Kaggle  
**File Size:** 61.27 MB  

### Data Profiling
- Checked `.head()`, `.info()`, `.describe()`  
- No nulls; duplicates removed  

### Data Wrangling
- Cleaned city names (accents, whitespace, capitalization)  
- Converted state abbreviations (e.g., SP → São Paulo)  
- Added `geolocation_city_clean` and `geolocation_state_full` for analysis  

### Data Table Schema
| Field | Data Type | Description |
|-------|-----------|------------|
| geolocation_zip_code_prefix | object | First 5 digits of zip code |
| geolocation_lat | float64 | Latitude |
| geolocation_lng | float64 | Longitude |
| geolocation_city | object | Original city name |
| geolocation_state | object | State abbreviation |
| geolocation_city_clean | object | Cleaned city name |
| geolocation_state_full | object | Full state name |

---

**Note:** Each dataset contains keys to merge with others, allowing a unified analysis workflow.

