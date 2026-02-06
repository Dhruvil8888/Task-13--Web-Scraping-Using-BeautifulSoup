# Task 13: Web Scraping Using BeautifulSoup

## 📌 Overview
This task demonstrates how to **collect data from web sources** using Python.  
It focuses on fetching HTML content, parsing it with **BeautifulSoup**, extracting useful information, and storing the data in a structured **CSV file**, while following **ethical web scraping practices**.

A scrape-friendly public website is used for demonstration purposes.

---

## 🛠 Tools Used
- Python  
- requests  
- BeautifulSoup (bs4)  
- VS Code  

---

## 📂 Project Files
- `web_scraper.py` – Web scraping script  
- `quotes.csv` – Extracted data stored in CSV format  

---

## 🔑 Key Concepts Covered
- HTTP requests using `requests`
- Parsing HTML with BeautifulSoup
- Identifying HTML tags and attributes
- Extracting text and links
- Handling missing tags safely
- Writing extracted data to CSV
- Ethical and responsible scraping

---

## ▶ Setup

Install required libraries:
```bash
pip install requests beautifulsoup4
Run the script:

python web_scraper.py
🧾 Source Code (web_scraper.py)
# TASK 13: Web Scraping Using BeautifulSoup
# This script scrapes quotes from a public, scrape-friendly website

import requests
from bs4 import BeautifulSoup
import csv

URL = "https://quotes.toscrape.com/"

def fetch_page(url):
    """
    Fetches HTML content from the given URL.
    """
    try:
        response = requests.get(url, timeout=10)
        if response.status_code == 200:
            return response.text
        else:
            print("Failed to fetch page. Status code:", response.status_code)
            return None
    except requests.exceptions.RequestException as e:
        print("Request error:", e)
        return None


def scrape_quotes(html):
    """
    Extracts quotes, authors, and tags from HTML content.
    """
    soup = BeautifulSoup(html, "html.parser")
    quotes_data = []

    quotes = soup.find_all("div", class_="quote")

    for quote in quotes:
        text = quote.find("span", class_="text")
        author = quote.find("small", class_="author")
        tags = quote.find_all("a", class_="tag")

        quote_text = text.text if text else "N/A"
        author_name = author.text if author else "N/A"
        tag_list = ", ".join([tag.text for tag in tags]) if tags else "N/A"

        quotes_data.append([quote_text, author_name, tag_list])

    return quotes_data


def save_to_csv(data, filename="quotes.csv"):
    """
    Saves extracted data to a CSV file.
    """
    with open(filename, "w", newline="", encoding="utf-8") as file:
        writer = csv.writer(file)
        writer.writerow(["Quote", "Author", "Tags"])
        writer.writerows(data)

    print(f"Data successfully saved to {filename}")


def main():
    print("Fetching webpage...")
    html = fetch_page(URL)

    if html:
        print("Scraping data...")
        data = scrape_quotes(html)
        save_to_csv(data)
    else:
        print("Scraping failed.")


if __name__ == "__main__":
    main()
📦 Deliverables
Web scraping script (web_scraper.py)

CSV file with extracted data (quotes.csv)

⚖ Ethical Scraping Practices
Used a publicly available, scrape-friendly website

No authentication bypassed

No excessive requests sent

Followed respectful request limits

🎯 Learning Outcome
After completing this task, the intern understands:

How web data is structured using HTML

How to extract data using BeautifulSoup

How to handle missing or optional HTML elements

How to store scraped data in CSV format

Ethical considerations in web scraping
