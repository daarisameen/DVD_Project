https://daarisameen.github.io/DVD_Project/

# The Order Journey: Comprehensive Marketplace Study & Analysis Report

**Project Title:** The Order Journey — An End-to-End Marketplace Data Visualization Study  
**Platform / Dataset:** Olist E-Commerce Marketplace (Brazil)  
**Study Scope:** 9 Relational Tables · 99,441 Orders · 96,096 Unique Buyers · 3,095 Sellers · 32,951 Products · 8,842 Seller Leads  
**Currency Standard:** Brazilian Real (R$ / BRL) — Total GMV: **R$ 15,843,553** (~R$ 15.84M BRL)  
**Deliverable Type:** Interactive Web Application & Analytics Report (`The_Order_Journey_Data_Visualization_Study.html`)

---

## 1. Executive Summary

In this project, we conducted a complete data analysis and visualization study tracking the full life cycle of an online order in Brazil's largest marketplace dataset (Olist). We followed the journey across every major operational touchpoint: merchant acquisition, catalog listing, customer discovery, checkout and installment payment, freight dispatch, and post-delivery customer satisfaction ratings.

### Key Headline Metrics:
* **Total Marketplace Revenue (GMV):** R$ 15.84 Million Brazilian Reais across 99,441 orders.
* **Order Fulfillment Rate:** **97.0%** of orders successfully delivered; only 1.24% lost before shipment (0.63% canceled, 0.61% unavailable).
* **Average Customer Rating:** **4.09 / 5.0 stars** (57.8% 5-star ratings, 11.5% 1-star ratings).
* **Delivery Performance:** **8.1% of delivered packages arrived late**. Average delivery transit took **12.6 days**.
* **The Satisfaction Cliff:** On-time orders average **4.29 stars**, while late orders collapse to **2.57 stars** (dropping further to **2.32 stars** if over 3 days late).
* **Customer Retention Challenge:** **96.9% of buyers purchased only once** (repeat buyer rate of just **3.12%**).
* **Basket Composition:** **90.1% of all orders contained only 1 single item**.
* **Geographic & Merchant Concentration:** São Paulo state represents **41.9% of all buyers** and **70.8% of all sellers**. The top **10% of sellers generate 67.5% of total sales**.

---

## 2. Currency & Financial Standard (Clarification)

A key finding in our data audit is the financial currency standard:
* **Currency Code:** Brazilian Real (**R$ / BRL**).
* **What R$ 15.8M means:** It represents **15,843,553 Brazilian Reais** in cumulative Gross Merchandise Value (GMV).
* **Clarification:** It is **neither US Dollars ($) nor Indian Rupees (₹ / Rs.)**. Because Olist operates in Brazil, all transaction values, line-item prices, freight fees, and payment amounts are in Brazilian Reais.

---

## 3. Data Foundations & Quality Assurance (Stage 00)

We connected nine relational tables joined on primary/foreign keys:

| Table Name | Entity Grain | Row Count | Primary Role & Features |
| :--- | :--- | :--- | :--- |
| `orders_dataset.csv` | 1 row per order | 99,441 | Purchase, approval, carrier shipping, customer delivery, and estimated delivery dates |
| `order_reviews_dataset.csv` | 1 row per review | 99,224 | Star ratings (1–5), survey creation dates, response times, review text/titles |
| `customers_dataset.csv` | 1 row per customer order | 99,441 | Customer ZIP prefix, city, state, and `customer_unique_id` (real person identity) |
| `geolocation_dataset.csv` | 1 row per raw address ping | 1,000,163 | Postal code prefix to latitude/longitude mapping for spatial plotting |
| `order_items_dataset.csv` | 1 row per basket item | 112,650 | Product ID, seller ID, item price, and shipping freight fee |
| `order_payments_dataset.csv` | 1 row per payment split | 103,886 | Payment method (credit card, boleto, voucher, debit) and installments |
| `products_dataset.csv` | 1 row per product SKU | 32,951 | Category name, weight (g), dimensions (L/H/W cm), listing photo count |
| `sellers_dataset.csv` | 1 row per seller | 3,095 | Seller postal code, city, and state locations |
| `marketing_leads` + `closed_deals` | 1 row per lead / won deal | 8,000 + 842 | Marketing origin channel, conversion dates, business segment, lead profile |

### Data Cleaning & Logic Checks We Conducted:
1. **Informative Missing Dates:** Missing delivery timestamps (~3% of rows) were verified to be structural: they correspond directly to non-delivered statuses (`canceled`, `shipped`, `unavailable`, `processing`). We filtered delivery lead-time analysis strictly to completed `delivered` orders.
2. **Customer Identity Disambiguation:** We proved that `customer_id` is generated afresh per order, whereas `customer_unique_id` identifies the actual individual. Using `customer_unique_id` prevented repeat shoppers from being counted as new customer acquisitions.
3. **Geolocation Deduplication:** Raw address pings contained duplicate coordinates and varied city spellings. We aggregated coordinates to the unique ZIP-code prefix level (calculating mean latitude/longitude and modal city names), achieving a **99.72% match rate** when joined to customer records.

---

## 4. Detailed Analytical Findings by Lifecycle Stage

```
   ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
   │  05. SELLER     │  ───> │  03. CATALOG    │  ───> │  04. PURCHASES  │
   │  ACQUISITION    │       │  & INVENTORY    │       │  & PAYMENTS     │
   │  (8,000 Leads)  │       │  (32,951 SKUs)  │       │  (112,650 Items)│
   └─────────────────┘       └─────────────────┘       └─────────────────┘
                                                                │
                                                                ▼
   ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
   │  06. 3D RISK    │  <─── │  01. FULFILLMENT│  <─── │  02. CUSTOMER   │
   │  LANDSCAPES     │       │  & SATISFACTION │       │  GEOGRAPHY      │
   │  (Multi-Var)    │       │  (99,441 Orders)│       │  (96,096 Users) │
   └─────────────────┘       └─────────────────┘       └─────────────────┘
```

### Stage 01: Fulfillment Performance & Customer Satisfaction
* **Order Funnel:** 97.0% of orders reach the customer safely. Pre-fulfillment loss accounts for only 1.24% of total demand.
* **Review Score Distribution:** The marketplace averages **4.09 stars**. 57.8% of reviews are 5 stars, while 11.5% are 1 star. 1-star ratings outnumber 2-star ratings by more than 3.5 to 1, demonstrating that dissatisfaction is driven by severe fulfillment failures rather than mild letdowns.
* **The Delivery Timing Cliff:** 
  * Early delivery (0–7+ days early) yields stable ratings of **4.13 to 4.32 stars**.
  * Minor delay (1–3 days late) drops ratings immediately to **3.77 stars**.
  * Moderate to severe delay (3–14+ days late) causes ratings to crash to **2.32 – 1.71 stars**.
* **On-Time Contrast:** 5-star orders have a **97.0% on-time delivery rate** (average transit: 10.7 days). In contrast, 1-star orders have only a **62.1% on-time delivery rate** (average transit: 21.3 days).
* **Order Timing Density:** Peak purchasing occurs on weekdays between **10:00 AM and 9:00 PM**, with sharp spikes at 1:00 PM (lunch break) and 8:00 PM (evening). Weekend order volume drops by approximately 30%.
* **Customer Voice as a Complaint Channel:** While only **35.9%** of 5-star reviewers write text comments, **76.5% of 1-star reviewers write lengthy complaints**.

### Stage 02: Customers, Geography & Retention
* **Geographic Core:** Customers are heavily concentrated in the Southeast macro-region (68.6% of all buyers). **São Paulo state alone represents 41.9% (41,746 buyers)**, followed by Rio de Janeiro (12.9%) and Minas Gerais (11.7%).
* **City Density:** São Paulo city is the largest urban market with 15,540 orders, more than double the second largest city (Rio de Janeiro at 6,882 orders).
* **The Retention Deficit:** **96.9% of unique customers purchased only once**. Only 3.12% placed two or more orders. Repeat buying rates remained uniformly low across all 5 macro-regions (2.5% to 3.2%), indicating that retention is a systemic marketplace issue rather than a regional one.

### Stage 03: Catalog, Inventory & Sellers
* **Catalog Leadership:** The largest product categories by SKU count are `bed_bath_table` (3,029 products), `sports_leisure` (2,867 products), and `furniture_decor` (2,657 products).
* **Revenue vs. Satisfaction Disconnect:** High-revenue categories such as `health_beauty` and `watches_gifts` maintain high customer review scores (4.15+ stars). However, bulky categories such as `bed_bath_table` and `office_furniture` suffer from lower ratings (<3.9 stars).
* **Physical Weight & Delay:** Product weight distributions reveal that furniture and home items have median weights exceeding 1,800g and long outlier tails extending to 15–30 kg, which creates logistics bottlenecks and shipping damage.
* **Listing Quality Independence:** Photo count and description length showed near-zero correlation with product physical dimensions (Pearson r ≈ 0.02 to 0.05), proving that listing quality is driven by seller diligence rather than category constraints.
* **Seller Concentration (Lorenz Curve):** **70.8% of active merchants operate out of São Paulo state**. The top **10% of sellers generate 67.5% of total GMV**, creating significant key-partner dependency.

### Stage 04: Purchases & Payment Behavior
* **Single-Item Baskets:** **90.1% of all marketplace orders contain exactly 1 item**. Orders containing 3 or more items account for less than 3% of total transactions.
* **Payment Preferences:**
  * **Credit Card:** 73.9% of transactions.
  * **Boleto Bancário (Bank Slip):** 19.0% of transactions.
  * **Vouchers & Gift Cards:** 5.6% of transactions.
  * **Debit Card:** 1.5% of transactions.
* **Installment Financing:** Over **52% of credit card shoppers split their payment across monthly installments** (frequently selecting 2, 3, 4, 6, 8, or 10 installments).

### Stage 05: Merchant Acquisition Funnel
* **Funnel Conversion:** Out of **8,000 Marketing Qualified Leads (MQLs)**, **842 signed on as active sellers** (overall conversion rate of **10.53%**).
* **Channel Effectiveness:** 
  * *Organic Search* generated the largest volume (2,296 leads) with an 11.2% conversion rate.
  * *Paid Search* was the highest-converting identified channel (**12.3% conversion rate**).
  * *Social / Email* converted at lower rates (7.0% – 8.5%).
  * *Unknown Origin* showed a 14.2% conversion rate, indicating attribution tracking gaps for returning warm leads.
* **Merchant Profile:** **69.7% of won sellers are third-party resellers**, while only 28.9% are direct manufacturers.
* **Sales Velocity:** The median sales cycle length is **13.0 days** from first contact to deal closure, with paid search closing fastest and email campaigns exhibiting long tails (>60 days).

### Stage 06: Interactive 3D Risk Landscapes
* **Category Risk Cube:** Plots average delay vs. review score vs. order volume vs. revenue. Clearly demonstrates that bulky, heavy goods occupy the high-delay, low-satisfaction risk quadrant.
* **Regional Risk Cube:** Plots late delivery percentage vs. average rating vs. customer volume across Brazilian states. Highlights that distant North/Northeast states suffer from elevated late delivery rates (15%–20%) and depressed review scores (3.6–3.8 stars).
* **Seller Performance Cube:** Plots merchant revenue vs. delay vs. satisfaction. Confirms that top-tier revenue merchants maintain fast dispatch times and high ratings (4.2+ stars), while fulfillment risks are concentrated in the fragmented tail of smaller sellers.

---

## 5. Summary of the Interactive Web Platform Features

The delivered visualization platform (`The_Order_Journey_Data_Visualization_Study.html`) was built with the following technical and UX enhancements:

1. **Light & Dark Theme Engine:**
   * **Default Theme:** Clean, high-contrast **Light Mode** (`[data-theme="light"]`) with smooth CSS transitions.
   * **Alternative Theme:** Modern **Dark Mode** (`[data-theme="dark"]`).
   * **Dynamic Switching:** One-click toggle in the sticky sidebar (`☀️ / 🌙`) with automatic chart palette recoloring and `localStorage` preference persistence.
2. **Chart Callouts under All 34 Visualizations:**
   * Every chart includes a structured callout block with:
     * `WHAT THIS CHART SHOWS`: Concise explanation of axes, chart type, and data points.
     * `WHAT WE FOUND`: Actionable, plain-English takeaway written in active team language (*"We found that...", "We analyzed..."*).
3. **Zero External Runtime Dependencies:**
   * Chart.js v4.5.1 and Plotly.js 3D are bundled directly into the standalone HTML file, guaranteeing instant rendering offline without CDN blocking.
4. **Responsive Multi-Chart Grid:**
   * Fluid 1-column, 2-column, 3-column, and 3D canvas layouts optimized for desktop, tablet, and mobile viewports.

---

## 6. Strategic Recommendations

Based on our empirical findings across the 9 tables, we recommend four immediate strategic initiatives:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                       STRATEGIC ACTION ROADMAP                              │
├──────────────────────────┬──────────────────────────┬───────────────────────┤
│ 1. LOGISTICS BUFFERING   │ 2. BASKET CROSS-SELLING  │ 3. RETENTION PROGRAM  │
│ Set realistic promise    │ Introduce automated      │ Launch post-purchase  │
│ dates & priority routes  │ bundles ("frequently     │ loyalty rewards to    │
│ to protect ratings.      │ bought together").       │ lift repeat purchases.│
└──────────────────────────┴──────────────────────────┴───────────────────────┘
```

1. **Protect the 1-Day Delay Margin:** Because customer ratings drop sharply the moment an order is 1 day late, the marketplace should adjust its estimated delivery algorithm to include a 1–2 day safety buffer. Under-promising and over-delivering protects the 5-star baseline.
2. **Specialized Freight Routing for Bulky Goods:** Heavy goods categories (`furniture_decor`, `bed_bath_table`) require dedicated freight contracts with white-glove couriers to eliminate transit damage and slow-delivery complaints.
3. **Basket Building & Product Bundling:** With 90.1% single-item orders, implementing automated recommendation engines (*"Frequently Bought Together"*, free freight thresholds for multi-item orders) can immediately increase Average Order Value (AOV) without additional customer acquisition costs.
4. **Customer Retention & Loyalty Programs:** Because 96.9% of customers currently buy only once, establishing automated post-purchase re-engagement campaigns (SMS/email coupons, category re-order reminders) represents the single highest-ROI growth opportunity for the marketplace.

---
*Report compiled from the verified dataset analysis of `The_Order_Journey_Data_Visualization_Study.html`.*
