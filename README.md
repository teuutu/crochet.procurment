# Crochet Procurement Classification Project  

## Overview  
This project analyzes crochet yarn purchases using **MySQL 8.0.44** and **Python**, focusing on suppliers **Alize Burkum** and **YarnArt**.  
It demonstrates skills in database design, SQL analytics, and Python visualization — blending engineering precision with creative flair.  

---

## Features  
- Relational schema for suppliers, categories, items, purchase orders, and order lines  
- Sample data for yarn purchases and supplier-country mapping  
- SQL queries for procurement classification and spend analysis  
- Python integration with `mysql-connector-python`, `pandas`, `seaborn`, and `matplotlib`  
- Bar chart visualization of spend by yarn category  

---

## Files Included  
```
crochet_procurement/
│
├── crochet_procurement.sql     # MySQL schema + sample data
├── crochet_procurement.py      # Python script for querying and visualization
├── requirements.txt            # Python dependencies
└── README.md                   # Project documentation
```

---

## Setup Instructions  

### 1. MySQL Setup  
Run the SQL dump to create and populate the database:  
```sql
SOURCE crochet_procurement.sql;
```

### 2. Python Environment  
Install dependencies:  
```bash
pip install -r requirements.txt
```

Contents of `requirements.txt`:  
```
mysql-connector-python==8.0.33
pandas==2.1.4
matplotlib==3.8.2
seaborn==0.13.0
```

---

## 📊 Python Analysis  
The Python script connects to MySQL, runs a join query across five tables, and visualizes total spend per yarn category:

```python
import mysql.connector
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

conn = mysql.connector.connect(
    host="localhost",
    user="root",
    password="Teodora12!",
    database="crochet_procurement"
)

query = """
SELECT s.name AS supplier, c.name AS category, i.name AS item,
       pl.quantity, pl.unit_price, (pl.quantity * pl.unit_price) AS total_spend
FROM po_lines pl
JOIN items i ON pl.item_id = i.item_id
JOIN categories c ON i.category_id = c.category_id
JOIN suppliers s ON i.supplier_id = s.supplier_id;
"""

df = pd.read_sql(query, conn)
category_summary = df.groupby('category')['total_spend'].sum().reset_index()

sns.barplot(data=category_summary, x='category', y='total_spend')
plt.title('Spend by Yarn Category')
plt.ylabel('EUR')
plt.show()
```

---

## Future Implementation Ideas  
- Add time-series analysis for monthly spend  
- Expand supplier list and item categories  
- Build interactive dashboards with Streamlit or Plotly  

---
