---
layout: post
title: "Web Scraping with scrapy in Python"
subtitle: "Scraping Pokemon Stackoverflow with Python"
background: '/img/posts/01.jpg'
---


# Web Scraping de Stackoverflow con Scrapy


```python
#importamos las librerias

from scrapy.item import Field
from scrapy.item import Item
from scrapy.spiders import Spider
from scrapy.selector import Selector
#from scrapy.loader.processors import MapCompose
from itemloaders import ItemLoader
#from bs4 import BeautifulSoup
from scrapy.crawler import CrawlerProcess
```

## Programa


```python

# ABSTRACCION DE DATOS A EXTRAER - DETERMINA LOS DATOS QUE TENGO QUE LLENAR Y QUE ESTARAN EN EL ARCHIVO GENERADO
class Pregunta(Item):
    id = Field()
    pregunta = Field()
    descripcion = Field()
    
```


```python
# CLASE CORE - SPIDER
class StackOverflowSpider(Spider):
    name = "MiPrimerSpider" # nombre, puede ser cualquiera 
    
    # Forma de configurar el USER AGENT en scrapy
    custom_settings = {
        'USER_AGENT': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/71.0.3578.80 Chrome/71.0.3578.80 Safari/537.36'
    }    

    # URL SEMILLA
    start_urls = ['https://stackoverflow.com/questions']


    # Funcion que se va a llamar cuando se haga el requerimiento a la URL semilla
    def parse(self, response):
        # Selectores: Clase de scrapy para extraer datos
        sel = Selector(response) 
        titulo_de_pagina = sel.xpath('//h1/text()').get()
        print (titulo_de_pagina)
        # Selector de varias preguntas
        preguntas = sel.xpath('//div[@id="questions"]//div[@class="question-summary"]') 
        i = 0
        for pregunta in preguntas:
            item = ItemLoader(Pregunta(), pregunta) # Instancio mi ITEM con el selector en donde estan los datos para llenarlo

            # Lleno las propiedades de mi ITEM a traves de expresiones XPATH a buscar dentro del selector "pregunta"
            item.add_xpath('pregunta', './/h3/a/text()') 
            item.add_xpath('descripcion', './/div[@class="excerpt"]/text()')
            item.add_value('id', i)
            i += 1
            yield item.load_item() # Hago Yield de la informacion para que se escriban los datos en el archivo

# EJECUCION EN TERMINAL:
# scrapy runspider 3_stackoverflow.py -o resultados.csv -t csv
        
```


```python
#bloque que ejecuta el spider en notebooks (sin terminal)
if __name__ == "__main__": # Código que se va a ejecutar al dar clic en RUN
    process = CrawlerProcess()
    process.crawl(StackOverflowSpider) # Nombre de la clase de mi Spider
    process.start()
```

    2021-09-06 22:24:22 [scrapy.utils.log] INFO: Scrapy 2.4.1 started (bot: scrapybot)
    2021-09-06 22:24:22 [scrapy.utils.log] INFO: Versions: lxml 4.6.3.0, libxml2 2.9.12, cssselect 1.1.0, parsel 1.5.2, w3lib 1.21.0, Twisted 21.2.0, Python 3.8.10 (default, May 19 2021, 11:01:55) - [Clang 10.0.0 ], pyOpenSSL 20.0.1 (OpenSSL 1.1.1k  25 Mar 2021), cryptography 3.4.7, Platform macOS-10.15.7-x86_64-i386-64bit
    2021-09-06 22:24:22 [scrapy.utils.log] DEBUG: Using reactor: twisted.internet.selectreactor.SelectReactor
    2021-09-06 22:24:22 [scrapy.crawler] INFO: Overridden settings:
    {'USER_AGENT': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, '
                   'like Gecko) Ubuntu Chromium/71.0.3578.80 Chrome/71.0.3578.80 '
                   'Safari/537.36'}
    2021-09-06 22:24:22 [scrapy.extensions.telnet] INFO: Telnet Password: 4974cebff33427aa
    2021-09-06 22:24:22 [scrapy.middleware] INFO: Enabled extensions:
    ['scrapy.extensions.corestats.CoreStats',
     'scrapy.extensions.telnet.TelnetConsole',
     'scrapy.extensions.memusage.MemoryUsage',
     'scrapy.extensions.logstats.LogStats']
    2021-09-06 22:24:22 [scrapy.middleware] INFO: Enabled downloader middlewares:
    ['scrapy.downloadermiddlewares.httpauth.HttpAuthMiddleware',
     'scrapy.downloadermiddlewares.downloadtimeout.DownloadTimeoutMiddleware',
     'scrapy.downloadermiddlewares.defaultheaders.DefaultHeadersMiddleware',
     'scrapy.downloadermiddlewares.useragent.UserAgentMiddleware',
     'scrapy.downloadermiddlewares.retry.RetryMiddleware',
     'scrapy.downloadermiddlewares.redirect.MetaRefreshMiddleware',
     'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware',
     'scrapy.downloadermiddlewares.redirect.RedirectMiddleware',
     'scrapy.downloadermiddlewares.cookies.CookiesMiddleware',
     'scrapy.downloadermiddlewares.httpproxy.HttpProxyMiddleware',
     'scrapy.downloadermiddlewares.stats.DownloaderStats']
    2021-09-06 22:24:22 [scrapy.middleware] INFO: Enabled spider middlewares:
    ['scrapy.spidermiddlewares.httperror.HttpErrorMiddleware',
     'scrapy.spidermiddlewares.offsite.OffsiteMiddleware',
     'scrapy.spidermiddlewares.referer.RefererMiddleware',
     'scrapy.spidermiddlewares.urllength.UrlLengthMiddleware',
     'scrapy.spidermiddlewares.depth.DepthMiddleware']
    2021-09-06 22:24:22 [scrapy.middleware] INFO: Enabled item pipelines:
    []
    2021-09-06 22:24:22 [scrapy.core.engine] INFO: Spider opened
    2021-09-06 22:24:22 [scrapy.extensions.logstats] INFO: Crawled 0 pages (at 0 pages/min), scraped 0 items (at 0 items/min)
    2021-09-06 22:24:22 [scrapy.extensions.telnet] INFO: Telnet console listening on 127.0.0.1:6023
    2021-09-06 22:24:23 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://stackoverflow.com/questions> (referer: None)
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     "            I'm trying to finish a project which sniffs and "
                     'injects packets into a game. This project contains the '
                     'classes: PacketSniffer (main), PacketInjector, and Logic.\n'
                     'The issue arrises when I try to call a ...\r\n'
                     '        '],
     'id': [0],
     'pregunta': ['Java - Unable to call method']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     '            I need to change the background color and '
                     'disable the button that is clicked in the following code. I '
                     'have tried to pass button in the command for the button but '
                     "it doesn't seem to work.\n"
                     'I tried ...\r\n'
                     '        '],
     'id': [1],
     'pregunta': ['Tkinter - Disable button on click made in a for loop']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     '            Bem, to tentando criar o jogo zombiedice, em '
                     'python. tenho algum progresso, que é isso:\n'
                     'regras: '
                     'http://www.sjgames.com/dice/zombiedice/img/ZDRules_English.pdf\n'
                     'import random\n'
                     "'numJog = int(input('Digite ...\r\n"
                     '        '],
     'id': [2],
     'pregunta': ['Creating the zombiedice game']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     '            When i subtract vector the resultant vector is '
                     'starting from origin, how can i map this result to the A to '
                     'B vector coordinates?\r\n'
                     '        '],
     'id': [3],
     'pregunta': ['Is there any function to map vector subtraction to its '
                  'coordinates?']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     "            I'm creating a Security group using terraform, "
                     "and when I'm running terraform plan. It is giving me an "
                     'error like some fields are required, and all those fields '
                     'are optional.\n'
                     'Terraform Version: v1.0.5\n'
                     '...\r\n'
                     '        '],
     'id': [4],
     'pregunta': ['Terraform: Inappropriate value for attribute "ingress" while '
                  'creating SG']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     '            Doing some Android development in Kotlin, can '
                     'anyone give me some pointers on how I can listen for a TCP '
                     'connection on one port and, once a client connects open a '
                     'new connection to a specific IP/port ...\r\n'
                     '        '],
     'id': [5],
     'pregunta': ['Android / Kotlin - How to connect 2 sockets together (poor '
                  "man's proxy)"]}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     '            I have a rather abstract problem that I thought '
                     'of but couldn’t figure out an efficient solution to it.\n'
                     'Problem\n'
                     'Suppose we have a list of rules represented by an array of '
                     'objects that all have an ...\r\n'
                     '        '],
     'id': [6],
     'pregunta': ['How do I rearrange an array to be ordered with compatible '
                  'neighbours?']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     '            using UnityEngine;\n'
                     '\n'
                     'public class Movement : MonoBehaviour\n'
                     '{\n'
                     '    public Rigidbody2D rb;\n'
                     '    public float speed = 500f;\n'
                     '    public float jumpforce = 700f;\n'
                     '    public Collider2D coll;\n'
                     '    public ...\r\n'
                     '        '],
     'id': [7],
     'pregunta': ['I recently started using unity 2D and I am having a '
                  'problem,when the character is in the air I am still able to '
                  'move it']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     '            I want to know under what conditions does '
                     '-[AVAsynchronousVideoCompositionRequest '
                     'sourceFramebytrackID] returns nil. I have a custom '
                     'compositor and when seeking AVPlayer, I find the method '
                     'sometimes ...\r\n'
                     '        '],
     'id': [8],
     'pregunta': ['AVAsynchronousVideoCompositionRequest sourceFramebytrackID '
                  'returns nil sometimes']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     '            I want to display the radio button input values '
                     '\u200b\u200bon the thymleaf page and save the radio button '
                     'input values.\n'
                     'However, as you can see below, my model class uses hashmap '
                     'to get input values(persons) \u200b\u200b...\r\n'
                     '        '],
     'id': [9],
     'pregunta': ['How to bind the hashmap to thymeleaf radio button using '
                  'th:field attribute in thymeleaf']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     "            Tweepy doesn't support bookmarks. I have saved "
                     'my bookmark links in a text file (about 400 links). I want '
                     'to get the media links of these tweets. I can do it for '
                     'tweets from timeline but while pulling ...\r\n'
                     '        '],
     'id': [10],
     'pregunta': ['Get media links from urls, not with api.user_timeline / '
                  'Tweepy-Python']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     "            I'm facing a deadlock in Python while trying to "
                     'synchronize multiple threads using a condition variable.\n'
                     'Here is an except of the code:\n'
                     'class Pipeline:\n'
                     '    def __init__():\n'
                     '        # results\n'
                     '        ...\r\n'
                     '        '],
     'id': [11],
     'pregunta': ['Python condition variable deadlock']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     '            I made a countdown timer that starts with 30 '
                     'seconds, and when it reaches zero seconds the player will '
                     'lose. Now what I want is if the player takes a powerup or '
                     'presses a certain key they will get ...\r\n'
                     '        '],
     'id': [12],
     'pregunta': ['How to add extra time for the countdown timer']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     '            I have a problem where a site runs okay on '
                     'hosting server browser but not on my local browser.\n'
                     'It is a site I need to migrate from a windows 2012 server to '
                     'a new windows 2019 server.  I have ensured ...\r\n'
                     '        '],
     'id': [13],
     'pregunta': ['IIS HTTP Error 404 local but not on server']}
    2021-09-06 22:24:23 [scrapy.core.scraper] DEBUG: Scraped from <200 https://stackoverflow.com/questions>
    {'descripcion': ['\r\n'
                     '            Suppose a processor has access to three levels '
                     'of memory. Level 1 has an access time\n'
                     'of 9 microseconds, level 2 has an access time of 23 '
                     'microseconds and level 3 has an\n'
                     'access time of 65 microseconds. ...\r\n'
                     '        '],
     'id': [14],
     'pregunta': ['Theory involving Processors']}
    2021-09-06 22:24:23 [scrapy.core.engine] INFO: Closing spider (finished)
    2021-09-06 22:24:23 [scrapy.statscollectors] INFO: Dumping Scrapy stats:
    {'downloader/request_bytes': 325,
     'downloader/request_count': 1,
     'downloader/request_method_count/GET': 1,
     'downloader/response_bytes': 38497,
     'downloader/response_count': 1,
     'downloader/response_status_count/200': 1,
     'elapsed_time_seconds': 0.776521,
     'finish_reason': 'finished',
     'finish_time': datetime.datetime(2021, 9, 6, 20, 24, 23, 477197),
     'item_scraped_count': 15,
     'log_count/DEBUG': 16,
     'log_count/INFO': 10,
     'memusage/max': 73232384,
     'memusage/startup': 73232384,
     'response_received_count': 1,
     'scheduler/dequeued': 1,
     'scheduler/dequeued/memory': 1,
     'scheduler/enqueued': 1,
     'scheduler/enqueued/memory': 1,
     'start_time': datetime.datetime(2021, 9, 6, 20, 24, 22, 700676)}
    2021-09-06 22:24:23 [scrapy.core.engine] INFO: Spider closed (finished)


    All Questions



```python

```


```python

```
