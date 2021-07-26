---
layout: post
title: "Web Scraping in Python"
subtitle: "Scraping Pokemon data."
background: '/img/posts/01.jpg'
---

## Writing to a .csv
Lastly, if we want to save the output, we can write the data to a csv



```{r setup, include=FALSE}
#Cargamos los paquetes de datos de R que vamos a usar
knitr::opts_chunk$set(echo = TRUE)
library(stringr)
library(tidyverse)
library(lubridate)
library(printr)
library(dplyr)
library(tokenizers)
library(nortest)
library(car)
library(lsr)
```


******
# Lectura del fichero y preparación de los datos
******

```{r,eval=TRUE,echo=TRUE}
#leemos los datos del archivo y verificamos que se han leído correctamente
claim <- read.csv('trainCLEAN.csv')
str(claim)
```

