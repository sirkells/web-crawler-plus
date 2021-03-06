# Web Crawler Plus


A micro-framework to crawl the web pages with crawlers configs. It can use MongoDB, Elasticsearch and Solr databases to 
cache and save the extracted data.


## Usage example with MongoDB as database

```python

from webcrawler_plus.parser import crawler
import json

example_config = json.load(open('../example.json'))

common_settings = {
    'COMPRESSION_ENABLED': False,
    'HTTPCACHE_ENABLED': True,
    'WCP_CRAWLER_COLLECTION': "weblinks",
    'WCP_CRAWLER_EXTRACTION_COLLECTION': "weblinks_extracted_data",
    'LOG_LEVEL': 'INFO'
}

mongodb_settings = {
    'PIPELINE_MONGODB_DATABASE': "crawler_data",
    'ITEM_PIPELINES': {'webcrawler_plus.pipelines.mongodb.MongoDBPipeline': 1},

    'HTTPCACHE_STORAGE': "webcrawler_plus.httpcache.mongodb.MongoDBCacheStorage",
    'HTTPCACHE_MONGODB_DATABASE': "crawler_data",
    "HTTPCACHE_MONGODB_PORT": 27017,
}

common_settings.update(mongodb_settings)

if __name__ == '__main__':
    crawler(config=example_config, settings=common_settings)

```

## Usage example with Elasticsearch as database

```python

from webcrawler_plus.parser import crawler
import json

example_config = json.load(open('../example.json'))
common_settings = {
    'COMPRESSION_ENABLED': False,
    'HTTPCACHE_ENABLED': True,
    'WCP_CRAWLER_COLLECTION': "weblinks",
    'WCP_CRAWLER_EXTRACTION_COLLECTION': "weblinks_extracted_data",
    'LOG_LEVEL': 'INFO'
}

es_settings = {
    'ITEM_PIPELINES': {'webcrawler_plus.pipelines.elasticsearch.ElasticsearchPipeline': 1},

    'HTTPCACHE_STORAGE': "webcrawler_plus.httpcache.elasticsearch.ESCacheStorage",
}

common_settings.update(es_settings)
print(common_settings)

if __name__ == '__main__':
    crawler(config=example_config, settings=common_settings)

```


## Using Solr as http cache storage

```bash

from webcrawler_plus.parser import crawl_website

common_settings = {
    'COMPRESSION_ENABLED': False,
    'HTTPCACHE_ENABLED': True,
    'WCP_CRAWLER_COLLECTION': "weblinks",
    'WCP_CRAWLER_EXTRACTION_COLLECTION': "weblinks_extracted_data",
    'LOG_LEVEL': 'INFO'
}

solr_settings = {

    'HTTPCACHE_SOLR_HOST': '127.0.0.1:8983',
    'HTTPCACHE_STORAGE': "webcrawler_plus.httpcache.solr.SolrCacheStorage",
}

common_settings.update(solr_settings)

if __name__ == '__main__':
    crawl_website(url="https://blog.github.com/",
                  settings=common_settings,
                  follow=True
                  )

```


### Using MongoDB as http cache storage


```python


settings = {
    'HTTPCACHE_ENABLED': True,
    'HTTPCACHE_STORAGE': "webcrawler_plus.httpcache.mongodb.MongoDBCacheStorage",
    'HTTPCACHE_MONGODB_DATABASE': "crawlers",
    "HTTPCACHE_MONGODB_PORT": 27017,
    'COMPRESSION_ENABLED': False,
}


```

### Using MongoDB to save extracted data

```python


settings = {
    'HTTPCACHE_STORAGE': "webcrawler_plus.httpcache.mongodb.MongoDBCacheStorage",
}

```


### Using Elasticsearch as http cache storage

```python


settings = {
    'HTTPCACHE_ENABLED': True,
    'HTTPCACHE_STORAGE': "webcrawler_plus.httpcache.elasticsearch.ESCacheStorage",
    'COMPRESSION_ENABLED': False
}

```

### Using Elasticsearch to save extracted data

```python


settings = {
    'HTTPCACHE_STORAGE': "webcrawler_plus.httpcache.elasticsearch.ESCacheStorage"
}

```

**Note:** By default, both ES and MongoDB uses `crawlers_data` as database and `weblinks` collection/doctype
to save the webpage data and `weblinks_extracted_data` as collection/doctype to save the extracted data.

All the entries in ES as url as id.


### Saving data to json file
```python


from webcrawler_plus.parser import crawler
import json
example_config = json.load(open('examples/example.json'))


settings = {
    'FEED_URI': 'result.json',

}

if __name__ == '__main__':
    crawler(config=example_config, settings=settings)


```


