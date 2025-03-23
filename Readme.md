
# News Scraper

### Overview

This project is a Python-based News Scraper that fetches the latest news articles from the BBC RSS feed, extracts their content, and saves them in a structured JSON file. It uses multi-threading to improve performance and includes error handling and logging.



## Features

- Fetches news article metadata from BBC's RSS feed.
- Uses newspaper3k to extract full article content.
- Implements multi-threading to scrape multiple articles efficiently.
- Saves extracted data in a structured JSON format.
- Includes error handling and logging for better debugging.



## Technologies Used

- Python

- feedparser (For RSS feed parsing)
- newspaper3k (For web scraping full articles)
- concurrent.futures (For multi-threading)
- json (For storing scraped data)
- logging (For error logging)

## Installation

### Prerequisites

Ensure you have Python installed (Python 3.x recommended). Install dependencies using:


```
pip install feedparser newspaper3k
```

## Output

- The script prints article details (title, author, date, content snippet).

- Scraped data is saved to articles.json.

- Errors (if any) are logged in news_scraper.log.