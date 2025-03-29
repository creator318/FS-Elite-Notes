#WebScraping/Scrapy 


Scrape the data from https://books.toscrape.com
And store the follwong data in MongoDB:
Database :  bookstore
Collection: books

Data to be extratcted and store in the following Document format:
==================================================================
```json
{
  "title": "The Coming Woman: A Novel Based on the Life of the Infamous Feminist, Victoria Woodhull",
  "price": "£17.93",
  "availability": "In",
  "rating": "Three",
  "category": "Books"
}
```

## Solution

spider.py
```python
import scrapy
from Books.items import QuoteItem

class QuotesSpider(scrapy.Spider):
  name = 'books_spider'
  start_urls = ['https://books.toscrape.com']

  def parse(self, response):
    quotes = response.css('div.quote')
    for q in quotes:
      item = QuoteItem()
      item['text'] = q.css('span.text::text').get()
      item['author'] = q.css('small.author::text').get()
      item['tags'] = q.css('div.tags a.tag::text').getall()
      yield item

    next_page = response.css('li.next a::attr(href)').get()
    if next_page:
      yield response.follow(next_page, callback=self.parse)
```

items.py
```python
import scrapy

class BookItem(scrapy.Item):
  title = scrapy.Field()
  price = scrapy.Field()
  availability = scrapy.Field()
  rating = scrapy.Field()
  category = scrapy.Field()
```

pipelines.py
```python
import pymongo
from scrapy.utils.project import get_project_settings

class MongoPipeline:
  def __init__(self):
    settings = get_project_settings()
    self.mongo_uri = settings.get("MONGO_URI")
    self.mongo_db = settings.get("MONGO_DB")
    self.collection_name = settings.get("MONGO_COLLECTION", "books")

  def open_spider(self, spider):
    self.client = pymongo.MongoClient(self.mongo_uri)
    self.db = self.client[self.mongo_db]

  def close_spider(self, spider):
    self.client.close()

  def process_item(self, item, spider):
    self.db[self.collection_name].insert_one(dict(item))
    return item
```

settings.py
```python

BOT_NAME = "bookscrape"

SPIDER_MODULES = ["bookscrape.spiders"]
NEWSPIDER_MODULE = "bookscrape.spiders"

ROBOTSTXT_OBEY = True

ITEM_PIPELINES = {
    'bookscrape.pipelines.MongoPipeline': 300,
}

MONGO_URI = 'mongodb://localhost:27017'
MONGO_DB = 'booksdb'
MONGO_COLLECTION = 'books'

TWISTED_REACTOR = "twisted.internet.asyncioreactor.AsyncioSelectorReactor"
FEED_EXPORT_ENCODING = "utf-8"
```