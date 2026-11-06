import os
import pandas as pd
import matplotlib.pyplot as plt

# Load and combine two months of data (from Day 14 pattern)
august = pd.read_csv("sales_august.csv")
september = pd.read_csv("sales_september.csv")
august["month"] = "August"
september["month"] = "September"
df = pd.concat([august, september], ignore_index=True)
df["revenue"] = df["quantity"] * df["price"]
print(f"Combined: {len(df)} rows | Total revenue: KES {df['revenue'].sum():,.0f}")

# Clean: standardize column names
# Clean: standardize column names - avoid duplicate 'region'
if "county" in df.columns:
    if "region" in df.columns:
        df = df.drop(columns=["region"])
    df = df.rename(columns={"county": "region"})

if "client_name" in df.columns:
    if "customer" in df.columns:
        df = df.drop(columns=["customer"])
    df = df.rename(columns={"client_name": "customer"})

# Transform: revenue category
def tier(rev):
    if rev >= 30000: return "High"
    elif rev >= 10000: return "Medium"
    else: return "Low"

customer_totals = df.groupby("customer")["revenue"].sum().reset_index()
customer_totals["tier"] = customer_totals["revenue"].apply(tier)
print(customer_totals["tier"].value_counts())

fig, axes = plt.subplots(1, 3, figsize=(15, 4))

# Chart 1: Revenue by region
region_rev = df.groupby("region")["revenue"].sum().sort_values(ascending=False)
region_rev.plot(kind="bar", ax=axes[0], color="#1F4E79")
axes[0].set_title("Revenue by Region (Aug-Sep)")

# Chart 2: Month-over-month comparison
monthly = df.groupby("month")["revenue"].sum()
monthly.plot(kind="bar", ax=axes[1], color=["#1F4E79", "#4472C4"])
axes[1].set_title("Revenue: August vs September")

# Chart 3: Customer tier distribution
customer_totals["tier"].value_counts().plot(kind="pie", ax=axes[2], autopct="%.0f%%")
axes[2].set_title("Customer Revenue Tiers")

plt.tight_layout()
os.makedirs("charts", exist_ok=True)
plt.savefig("charts/project_analysis.png", dpi=150)
print("Charts saved.")
plt.show()