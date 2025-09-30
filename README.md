# 🕸️ Scraping Data from a Real Website + Pandas

## 📌 Project Overview

This project demonstrates how to scrape real-world data from a live website (Wikipedia) using **Python** libraries like `requests` and `BeautifulSoup`, then process and analyze the data with **Pandas**.

The target dataset is the **list of the largest companies in the United States by revenue**, extracted from:
🔗**[Wikipedia - List of largest companies in the United States by revenue](https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue)

The workflow includes fetching the web page, parsing HTML tables, cleaning the extracted data, converting it into a Pandas DataFrame, and finally saving the results into a CSV file for further use.

---

## ⚙️ Technologies Used

- **Python**
- **Requests** → fetch web pages
- **BeautifulSoup (bs4)** → parse HTML content
- **Pandas** → clean and structure tabular data
- **Excel / CSV** → export final dataset

---

🚀 How It Works

1. Import Required Libraries

```
from bs4 import BeautifulSoup
import requests
import pandas as pd
```

---

2. Send HTTP Request to Wikipedia

```
url = 'https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue'
headers = {"User-Agent": "Mozilla/5.0"}
page = requests.get(url, headers=headers)
print(page)   # <Response [200]> means success
```

---

3. Parse the Webpage with BeautifulSoup

```
soup = BeautifulSoup(page.text, 'html')
table = soup.find('table', class_='wikitable sortable')
```

---

4. Extract Table Headers

```
world_titles = table.find_all('th')
world_table_titles = [title.text.strip() for title in world_titles]
print(world_table_titles)
# ['Rank', 'Name', 'Industry', 'Revenue (USD millions)', 'Revenue growth', 'Employees', 'Headquarters']
```

### Initialize DataFrame:

```
df = pd.DataFrame(columns=world_table_titles)
```

---

5. Extract Table Rows

```
column_data = table.find_all('tr')

for row in column_data[1:]:
    row_data = row.find_all('td')
    individual_row_data = [data.text.strip() for data in row_data]
    length = len(df)
    df.loc[length] = individual_row_data
```

### ✅ Example row extracted:

```
['1', 'Walmart', 'Retail', '680,985', '5.1%', '2,100,000', 'Bentonville, Arkansas']
```

6. Clean the Data

```
# Convert revenue and employees to integers
df['Revenue (USD millions)'] = df['Revenue (USD millions)'].str.replace(',', '').astype(int)
df['Employees'] = df['Employees'].str.replace(',', '').astype(int)

# Convert percentage to float
df['Revenue growth'] = df['Revenue growth'].str.replace('%', '').astype(float)
```

7. Export to CSV

```
df.to_csv(r'C:\Users\name\my_notebook.ipynb\Python Web Scraping Project\Companies.csv', index=False)
```

## 📊 Sample Output

| Rank | Name    | Industry | Revenue (USD millions) | Revenue growth | Employees | Headquarters          |
| ---- | ------- | -------- | ---------------------- | -------------- | --------- | --------------------- |
| 1    | Walmart | Retail   | 680,985                | 5.1%           | 2,100,000 | Bentonville, Arkansas |
| 2    | Amazon  | Retail   | 637,959                | 11.0%           | 1,556,000 | Seattle, Washington   |

## 💾 Files

- **Scraper Script** → Python notebook or `.py` file containing scraping logic.
- **Companies.csv** → Exported dataset of scraped companies.

## 📚 Key Learning Points

- How to scrape structured data (tables) from a live website.
- Cleaning raw HTML data before analysis.
- Organizing web-scraped data into a structured **Pandas DataFrame**.
- Exporting scraped data to CSV for visualization and reporting.
