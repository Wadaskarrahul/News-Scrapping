```python
import feedparser
from newspaper import Article

```


```python
import json
```


```python
# Help in debugging error without breaking execution
import logging
import concurrent.futures # multithereding
logging.basicConfig(filename='news_scraper.log',level=logging.INFO, format= "%(asctime)s-%(levelname)s - %(message)s") 
```


```python
rss_url = "http://feeds.bbci.co.uk/news/rss.xml"
```


```python
def fetch_rss_feeds(rss_url):
    try:
        feeds = feedparser.parse(rss_url)
        if not feeds.entries:
            logging.warning("No articles found in RSS feeds.")
        return feeds
    except Exception as e:
        logging.error(f"Error fetching rss feeds:{e}")
        return None
```


```python
feeds = fetch_rss_feeds(rss_url)
for entry in feeds.entries:
    print(f"Title:{entry.title}")
    print(f"Link:{entry.link}")
    print()
```

    Title:Civil Service told to slash running costs by 15%
    Link:https://www.bbc.com/news/articles/cy5nzy403l0o
    
    Title:Pope Francis to be discharged from hospital
    Link:https://www.bbc.com/news/articles/crrdv84rg4do
    
    Title:Trump envoy dismisses Starmer plan for Ukraine
    Link:https://www.bbc.com/news/articles/c62zm4eqvp7o
    
    Title:Grassroots anger tests Farage's grip on Reform UK
    Link:https://www.bbc.com/news/articles/c8x4np7zkx9o
    
    Title:Owners shocked as dogs seized for XL bully checks
    Link:https://www.bbc.com/news/articles/cgj5wng9y9lo
    
    Title:The man with a mind-reading chip in his brain - thanks to Elon Musk
    Link:https://www.bbc.com/news/articles/cewk49j7j1po
    
    Title:UK TV industry in crisis, says Wolf Hall director
    Link:https://www.bbc.com/news/articles/c3w10816en3o
    
    Title:A life spent waiting - and searching rows of unclaimed bodies
    Link:https://www.bbc.com/news/articles/c15qyyzz89lo
    
    Title:'My husband is a fighter pilot in Ukraine. Here's how I really feel about a ceasefire'
    Link:https://www.bbc.com/news/articles/c70wgq7y11qo
    
    Title:'Wonderful teenagers helped my son on Halloween': Readers recall kindness of strangers
    Link:https://www.bbc.com/news/articles/ckg14rxnw8jo
    
    Title:End of hedonism? Why Britain turned its back on clubbing
    Link:https://www.bbc.com/news/articles/czed9321l37o
    
    Title:Fear and anger mount as 'battle for the soul of Romanian democracy' looms
    Link:https://www.bbc.com/news/articles/c4g7w3v5vw7o
    
    Title:Your pictures on the theme of 'my best photo'
    Link:https://www.bbc.com/news/articles/c871q3dpyxpo
    
    Title:Are Nigerians abroad widening the class divide back home?
    Link:https://www.bbc.com/news/articles/cvg1p5ek72vo
    
    Title:Urgent investigation ordered into power outage that closed Heathrow Airport
    Link:https://www.bbc.com/news/articles/cgm1krkxrxgo
    
    Title:The Papers: 'Reeves wields axe on Civil Service' and 'boxing says bye George'
    Link:https://www.bbc.com/news/articles/cgj5qv862d9o
    
    Title:Government considering sending failed asylum seekers to Balkans
    Link:https://www.bbc.com/news/articles/cddyj8ge08po
    
    Title:MS Dhoni: The 43-year-old Indian cricket icon gears up for another IPL
    Link:https://www.bbc.com/news/articles/c5y2z448e8xo
    
    Title:Israel strikes Lebanon after first rocket attack since ceasefire
    Link:https://www.bbc.com/news/articles/cn4ynpzk8d8o
    
    Title:Facebook to stop targeting ads at UK woman after legal fight
    Link:https://www.bbc.com/news/articles/c1en1yjv4dpo
    
    Title:Reeves says she will not 'tax and spend' ahead of Spring Statement
    Link:https://www.bbc.com/news/articles/c78eg7dp9ypo
    
    Title:Protesters call for 'rights, justice' after Istanbul mayor's arrest
    Link:https://www.bbc.com/news/articles/c0egjvj8vdro
    
    Title:Police burn chemicals as running event cancelled
    Link:https://www.bbc.com/news/articles/cz614e6eepgo
    
    Title:High-rise flats demolition to alter Glasgow skyline
    Link:https://www.bbc.com/news/articles/c93n44x473go
    
    Title:McCann retires as British MMA icon
    Link:https://www.bbc.com/sport/mixed-martial-arts/articles/cn89e7yzg49o
    
    Title:Police release new evidence in timeline of Hackman and his wife's death
    Link:https://www.bbc.com/news/articles/cx2g1xvzg4ko
    
    Title:Why was a white balloon flying across the UK?
    Link:https://www.bbc.com/news/articles/clyrpyk5re8o
    
    Title:Killer who cut up housemate caught in chance sighting
    Link:https://www.bbc.com/news/articles/cy4vxx4x74ko
    
    Title:BBC News app
    Link:https://www.bbc.co.uk/news/10628994
    
    Title:The Big Rachel Reeves Weekend
    Link:https://www.bbc.co.uk/sounds/play/p0kzsh79
    
    Title:Is Donald Trump ready to defy the courts?
    Link:https://www.bbc.co.uk/sounds/play/p0kz4qqr
    
    Title:England begin bid for legacy-defining year
    Link:https://www.bbc.com/sport/rugby-union/articles/cn8rng8g8ppo
    
    Title:'Mr Calm' Bellamy looks to Skopje after opening win
    Link:https://www.bbc.com/sport/football/articles/c3rn00ylyr0o
    
    Title:Draper shocked by Mensik in Miami Open second round
    Link:https://www.bbc.com/sport/tennis/articles/c1lp13d2gnno
    
    Title:Brady upsets Edwards with dominant display
    Link:https://www.bbc.com/sport/mixed-martial-arts/articles/cwyd45wnnkpo
    
    Title:McCann retires as British MMA icon
    Link:https://www.bbc.com/sport/mixed-martial-arts/articles/cn89e7yzg49o
    
    Title:Scotland triumph in thriller against Wales
    Link:https://www.bbc.com/sport/rugby-union/videos/cq8y1w94dzwo
    
    


```python
def scrape_article(url):
    try:
        article = Article(url)
        article.download()
        article.parse()
        return {
            "title": article.title,
            "authors":article.authors,
            "publish_date":str(article.publish_date)if article.publish_date else "unknown",
            "text":article.text[:500]
        }
    except Exception as e:
        logging.error(f"Error scraping article{url}:{e}")
        return None

```


```python
for entry in feeds.entries[:3]:
    article = scrape_article(entry.link)
    if article: # Ensure article scrapping was successful
        print("="*50)
        print(f"Title :{article.get('title','N/A')}")
        print(f"Authors : {','.join(article.get('authors',[])) if article.get('authors') else 'Unknown'}")
        print(f"Publish Date: {article.get('publish_date','unknown')}")
        print(f"Text: {article.get('text', 'No content available')[:200]}")
        print("="*50)
        print()
    else:
        logging_error(f"failed to scrape article: {entry.link}")
```

    ==================================================
    Title :Civil Service told by government to slash running costs by 15%
    Authors : Unknown
    Publish Date: unknown
    Text: Civil Service told to slash running costs by 15%
    
    The changes are part of the government's ongoing spending review, the BBC understands, with Chancellor Rachel Reeves set to deliver her Spring Stateme
    ==================================================
    
    ==================================================
    Title :Pope Francis to be discharged from hospital on Sunday
    Authors : Unknown
    Publish Date: unknown
    Text: Pope Francis to be discharged from hospital
    
    Pope Francis (file image) has been battling double pneumonia for more than five weeks
    
    Pope Francis was never intubated and always remained alert and orien
    ==================================================
    
    ==================================================
    Title :Trump envoy Steve Witkoff dismisses Starmer plan for Ukraine
    Authors : Unknown
    Publish Date: unknown
    Text: Trump envoy dismisses Starmer plan for Ukraine
    
    8 hours ago Share Save James Landale • @BBCJLandale Diplomatic correspondent Reporting from Kyiv Share Save
    
    Reuters Sir Keir met a group of military le
    ==================================================
    
    


```python
# articles_data =[]
# for entry in feeds.entries:
#     article = scrape_article(entry.link)
#     articles_data.append({
#         'title' : article.title,
#         'authors': article.authors,
#         'publish_date' : str(article.publish_date),
#         'text': article.text
#     })
articles_data = []
with concurrent.futures.ThreadPoolExecutor(max_workers=5) as executor:  # Correct spelling
    future_to_url = {executor.submit(scrape_article, entry.link): entry for entry in feeds.entries}  # Fixed typo
    for future in concurrent.futures.as_completed(future_to_url):
        result = future.result()
        if result:
            articles_data.append(result)
    
```


```python
def save_to_file(data, filename):
    try:
        with open(filename, "w") as f:  # Correct indentation
            json.dump(data, f, indent=4)  # Proper indentation inside 'with' block
        logging.info(f"Data saved successfully to {filename}")
    except Exception as e:  # Fixed 'expect' -> 'except'
        logging.error(f"Error saving data to file: {e}")
```


```python
save_to_file(articles_data,'articles.json')
print("Data saved to article.json")
```

    Data saved to article.json
    


```python

```


```python

```
