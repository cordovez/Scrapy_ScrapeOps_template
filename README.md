# Scrapy_ScrapeOps_template
A Scrapy spider project for websites that respond with a 403 HTTP response.

To bypass a 403 response  [I followed instructions from this video](https://www.youtube.com/watch?v=NiFuoJw0sn8) to create these changes:

## Settings 
### Middlewares:
```
DOWNLOADER_MIDDLEWARES = {
    "scrapy.downloadermiddlewares.useragent.UserAgentMiddleware": None,
    "scrapy_user_agents.middlewares.RandomUserAgentMiddleware": 400,
}
```
### Concurrent Requests:
```
CONCURRENT_REQUESTS = 1
```

## Spider
The function get_proxy_url() to use ScrapeOps, and start_requests() to override scrapy's default.
## connect to ScrapeOps
``` py
def get_proxy_url(url):
    payload = {"api_key": API_KEY, "url": url}
    proxy_url = "https://proxy.scrapeops.io/v1/?" + urlencode(payload)
    return proxy_url
```

## Override start_urls
```py
class MySpiderSpider(scrapy.Spider):
    name = "my_spider"
    allowed_domains = ["www.your-url-with-403-response.com"]
    # start_urls = ["https://www.your-url-with-403-response.com"]

    def start_requests(self):
        start_url = "https://www.your-url-with-403-response.com/page/items"
        yield scrapy.Request(url=get_proxy_url(start_url), callback=self.parse)

    def parse(self, response):
        pass

```