---
layout: post
title: "Web Scraping in Python"
subtitle: "Scraping Pokemon data."
background: '/img/posts/01.jpg'
---

# Web Scraping de Stackoverflow


```python
#importamos las librerias

from scrapy.item import Field
from scrapy.item import Item
from scrapy.spiders import Spider
from scrapy.selector import Selector
#from scrapy.loader import ItemLoader
from itemloaders import ItemLoader
```


```python

```


```python
#clase que hereda de item
class Pegunta(Item):
    #propiedades que voy a extraer
    id = Field()
    pregunta = Field()
    descripcion = Field()
    
```


```python
#Hereda de la clase Spider
class StackOverflowSpider(Spider):
    name = "MiSpider"
    #defino mi user agent
    custom_settings = {
        'USER_AGENT': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/71.0.3578.80 Chrome/71.0.3578.80 Safari/547.36'
    }
    #url semilla en la que realizamos la extraccion
    start_urls = ['https://stackoverflow.com/questions']
    
    #funcion que realiza el parseo
    def parse(self, response):
        sel = Selector(response)
        preguntas = sel.xpath('//div[@ide="questions"]//div[@class="question-summary"]')
        
        for pregunta in preguntas:
            item = ItemLoader(Pregunta(), pregunta)
            item.add_xpath('pregunta', './/h3/a/text')
            item.add_xpath('descripcion', './/div[@class="excerpt"]/text()')
            item.add_value('id', 1)
            
            yield item.load_item()
        
```


```python

```


```python

```


```python

```
