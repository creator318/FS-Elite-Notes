#WebScraping/Scrapy

### 1. Which line extracts all tag texts for a quote?
- ✅  `q.css('div.tags a.tag::text').getall()`
- ❌ `q.css('a.tag::href').getall()`
- ❌ `q.css('div.tags a.tag').get()`
- ❌ `q.select('tags').all()`

### 2. Why is dict(item) used before inserting into MongoDB?
- ✅ Items are not serializable directly
- ❌ To convert the response to JSON
- ❌ To remove metadata
- ❌ MongoDB only accepts strings

### 3. Which CSS selector correctly extracts quote text?
- ✅  `span.text::text`
- ❌ `div.quote-text::value`
- ❌ `div.author`
- ❌ `a.tag[href]`

### 4. Why might this CSS selector return None? 
```css
quote.css('div.text::text').get()
```
- ❌ The element doesn’t exist
- ✅ Quotes use span.text, not div.text
- ❌ The site uses JavaScript rendering
- ❌ You forgot to install BeautifulSoup

### 5. In Scrapy, what does the start_urls attribute do?
- ❌ Stores downloaded pages
- ❌ Defines URLs for MongoDB
- ✅ Automatically triggers the parse() method for each URL
- ❌ Registers middleware

### 6. You run `db.quotes.find({ rating: 'Five' }).count()` and get 0. What is a likely reason?
- ❌ Ratings are stored as numbers
- ❌ You used the wrong MongoDB URI
- ❌ Scrapy removes the rating before storing
- ✅ The rating field wasn’t scraped properly or pipeline was skipped

### 7. What is the purpose of defining an Item class in Scrapy?
- ❌ Stores request headers
- ❌ Replaces the parse() method
- ✅ Provides structured data containers
- ❌ Automatically maps to MongoDB

### 8. What happens if the next_page selector returns None?
- ❌ Scrapy skips the URL
- ✅ The crawl ends gracefully
- ❌ Scrapy restarts from the first page
- ❌ Scrapy throws an error

### 9. What is the primary role of the Scrapy Engine?
- ❌ Manage HTML parsing
- ❌ Render JavaScript-based pages
- ❌ Send data to MongoDB
- ✅ Coordinate requests, responses, and items between components

### 10. Which setting enables a custom pipeline in Scrapy?
- ❌ SPIDER_PIPELINE
- ✅ ITEM_PIPELINES
- ❌ MONGO_PIPELINE
- ❌ ITEM_STORAGE_BACKEND

### 11. Where is the MongoDB connection usually initialized in a Scrapy pipeline?
- ❌ `parse()`
- ❌ `__init__()`
- ❌ `settings.py`
- ✅  `open_spider()`

### 12. What is the result of calling `insert_one()` in `process_item()` but forgetting to close the client in `close_spider()`?
- ❌ MongoDB auto-closes everything safely
- ✅ It works, but may cause a memory leak or leave open connections
- ❌ Data will not be written at all
- ❌ Scrapy crashes

### 13. Which method in a Spider is called for each response?
- ❌ `yield()`
- ❌ `scrape()`
- ✅  `parse()`
-  ❌ `start_requests()`

### 14. Which of the following can be returned from a Spider’s `parse()` method?
- ❌ Only Items
- ✅ Either Items or Requests
- ❌ Only Response objects
- ❌ Only Requests

### 15. What will happen if you forget to return the item in `process_item()`?
- ❌ It crashes
- ❌ Scrapy skips it
- ❌ MongoDB inserts an empty document
- ✅ Data will not pass to next pipeline or output