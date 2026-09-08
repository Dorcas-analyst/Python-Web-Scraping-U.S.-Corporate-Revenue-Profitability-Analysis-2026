# Python-Web-Scraping-U.S.-Corporate-Revenue-Profitability-Analysis-2026
This project was more than an exercise in extracting tables from a webpage. It demonstrated how a data analyst can move from unstructured web content → structured data → data validation → analysis → visualization → business insight.

## Project Overview
This project demonstrates an end-to-end web scraping and data analysis workflow using Python.
The objective was to extract and structure information about some of the largest companies in the United States from a publicly accessible Grokipedia webpage. The extracted data was transformed from HTML tables into pandas DataFrames and used for analysis and visualization.

Source: Grokipedia — List of largest companies in the United States by revenue
Tools: Python, Requests, BeautifulSoup4, Pandas

## Project Objectives
- Retrieve webpage content using Python.
- Parse HTML using BeautifulSoup.
- Identify and extract HTML tables.
- Convert scraped data into pandas DataFrames.
- Clean and structure extracted values.
- Validate scraped datasets.
- Identify and troubleshoot extraction errors.
- Analyze company revenue, profitability, and industry distribution.
- Create visualizations from the cleaned datasets

## Dataset
The source webpage contained three major tables:
1. Table 1	Top 10 U.S. companies by revenue
2. Table 2	Top 50 U.S. companies by net profit
3. Table 3	Estimated revenue and founder/owner information

The first two tables were successfully extracted and used for analysis.

## Project Workflow
Webpage
   ↓
HTTP Request
   ↓
HTML Parsing
   ↓
Table Identification
   ↓
Header & Row Extraction
   ↓
Data Cleaning
   ↓
Data Validation
   ↓
Exploratory Analysis
   ↓
Visualization
   ↓
Business Insights

## Technologies Used
- Python (Requests, BeautifulSoup4, Pandas, HTML Parsing, Web Scraping)
- Data Cleaning
- Data Validation
- Data Visualization

1. Fetching the Webpage
import requests
from bs4 import BeautifulSoup
import pandas as pd
  
url = "https://grokipedia.com/page/List_of_largest_companies_in_the_United_States_by_revenue"

page = requests.get(url)
soup = BeautifulSoup(page.text, 'html')

The HTML was inspected using BeautifulSoup to understand the structure of the webpage and identify the available tables.

2. Extracting the Tables
table1 = soup.find_all('table')[0]
table2 = soup.find_all('table')[1]
table3 = soup.find_all('table')[2]

The tables represented:
Revenue ranking
Net-profit ranking
Estimated revenue/founder-owner information

3. Extracting and Cleaning Headers
table_header = table2.find_all('th')

world_table_header = [title.text for title in table_header]

df = pd.DataFrame(columns=world_table_header)

Extracting the text from the <th> elements allowed the DataFrame schema to reflect the source webpage.

4. Extracting Rows
column_data = table2.find_all('tr')

for row in column_data[1:]:
    row_data = row.find_all('td')
    individual_row_data = [data.text.strip() for data in row_data]

    df.loc[len(df)] = individual_row_data

This extraction method successfully produced the Top 50 Net Profit dataset.

The same approach was used to extract the Top 10 Revenue dataset.

5. Data-Quality Issue

One of the most important findings was an error in the extraction logic for Table 3.

Problematic logic
column_data3 = table3.find_all('td')

for row in column_data3[1:]:
    row_data in row.find_all('td')

Two problems were present:

find_all('td') collected individual cells instead of complete rows.
in was used instead of the assignment operator =.

This resulted in 69 duplicate records rather than the expected distinct founder/owner records.

Corrected logic
column_data3 = table3.find_all('tr')

for row in column_data3[1:]:
    row_data = row.find_all('td')
    individual_row_data = [data.text.strip() for data in row_data]

    df.loc[len(df)] = individual_row_data

This demonstrated why output validation is essential in data scraping.

## Key Findings
1. Revenue vs Profitability: Amazon was among the largest companies by revenue but did not rank among the top five companies by net profit, demonstrating that high revenue does not necessarily translate into the highest profitability.
2. Industry Representation: Financials was the most represented industry in the Top 50 net-profit dataset, followed by Information Technology and Communication Services.
3. Profitability Outlier: MicroStrategy appeared as a notable outlier, with reported revenue substantially lower than its reported net profit. This was flagged as an observation requiring caution when interpreting the dataset.

## Visualizations
The project produced three visualizations:
1. Top 10 U.S. Companies by Revenue
2. Industry Distribution Across Top 50 Companies by Net Profit
3. Net Profit vs Revenue

These charts helped communicate differences in corporate scale, profitability, and industry concentration.

## Key Lessons
- Web Scraping: Web scraping requires an understanding of HTML structure, not just Python syntax.
- Data Validation: A program running without an exception does not guarantee correct data.
- Debugging: Small coding mistakes can silently produce large data-quality problems.
- Robust Scraping: Hard-coded table indexes can make a scraper fragile if the webpage structure changes.
- Responsible Scraping: Production scraping should consider HTTP status checks, request delays, descriptive User-Agent headers, robots.txt, and the target website's terms of service.

## Reference
1, Source: Grokipedia — List of largest companies in the United States by revenue
2, Libraries: Requests, BeautifulSoup4, Pandas

## Conclusion
This project demonstrated a complete Python web scraping pipeline, from retrieving and parsing a live webpage to extracting structured tables, validating the results, identifying data-quality problems, and generating analytical insights.
The most important takeaway was that data extraction is only the beginning. A reliable analyst must validate the resulting dataset before using it for analysis, reporting, or decision-making.

## Keywords
Python Web Scraping BeautifulSoup Requests Pandas Data Analysis Data Cleaning Data Validation HTML Parsing Data Visualization Data Science
