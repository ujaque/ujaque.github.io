---
layout: post
title: "Analisis de mis visulaizaciones de Netflix"
subtitle: "Scraping Pokemon Stackoverflow with Python"
background: '/img/posts/01.jpg'
---

# ANALISIS DE CONSUMO DE NETFLIX CON PYTHON

## 1. IMPORTACION Y CARGA DE DATOS


```python
from google.colab import files
datos = files.upload()
```



<input type="file" id="files-4a70230d-33a5-47ae-8143-b7f87dd836d7" name="files[]" multiple disabled
   style="border:none" />
<output id="result-4a70230d-33a5-47ae-8143-b7f87dd836d7">
 Upload widget is only available when the cell has been executed in the
 current browser session. Please rerun this cell to enable.
 </output>
 <script src="/nbextensions/google.colab/files.js"></script> 


    Saving NetflixViewingHistory.csv to NetflixViewingHistory.csv



```python
import io
import pandas as pd

#cargamos los datos del csv a df
df = pd.read_csv(io.BytesIO(datos['NetflixViewingHistory.csv']))
```

## 2. ANALISIS EXPLORATORIO


```python
df.shape
#verificamos cuantas filas y cuantas columnas tenemos
```




    (1960, 2)




```python
df.info()
#aplicamos el metodo info que nos da informacion sobre la variable. vemos que tenemos 1960 elementos no nulos
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1960 entries, 0 to 1959
    Data columns (total 2 columns):
     #   Column  Non-Null Count  Dtype 
    ---  ------  --------------  ----- 
     0   Title   1960 non-null   object
     1   Date    1960 non-null   object
    dtypes: object(2)
    memory usage: 30.8+ KB



```python
df.head(50)
#visualizamos los 50 primeros casos, para poder ver el contenido de nuestro dataframe
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>The Good Doctor: Temporada 1: Sonrisa</td>
      <td>12/9/21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Good Doctor: Temporada 1: Dolor</td>
      <td>12/9/21</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Un ...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Gra...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BattleBots: Peleas de robots: Temporada 2: La ...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>5</th>
      <td>BattleBots: Peleas de robots: Temporada 2: A p...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>6</th>
      <td>BattleBots: Peleas de robots: Temporada 2: El ...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>7</th>
      <td>BattleBots: Peleas de robots: Temporada 2: La ...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>8</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Est...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>9</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Va ...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>10</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Rob...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>11</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Rob...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>12</th>
      <td>BattleBots: Peleas de robots: Temporada 2: El ...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>13</th>
      <td>BattleBots: Peleas de robots: Temporada 1: Sol...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>14</th>
      <td>BattleBots: Peleas de robots: Temporada 1: Los...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>15</th>
      <td>BattleBots: Peleas de robots: Temporada 1: Últ...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>16</th>
      <td>BattleBots: Peleas de robots: Temporada 1: La ...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>17</th>
      <td>BattleBots: Peleas de robots: Temporada 1: Pri...</td>
      <td>10/9/21</td>
    </tr>
    <tr>
      <th>18</th>
      <td>BattleBots: Peleas de robots: Temporada 1: La ...</td>
      <td>9/9/21</td>
    </tr>
    <tr>
      <th>19</th>
      <td>La casa de papel: Parte 5: Vivir muchas vidas</td>
      <td>9/9/21</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Al borde: Temporada 1: Hace unos dos meses</td>
      <td>9/9/21</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Bolívar: Temporada 1: Episodio 1</td>
      <td>9/9/21</td>
    </tr>
    <tr>
      <th>22</th>
      <td>La casa de papel: Parte 5: Tu sitio en el cielo</td>
      <td>8/9/21</td>
    </tr>
    <tr>
      <th>23</th>
      <td>The Good Doctor: Temporada 1: De corazón</td>
      <td>7/9/21</td>
    </tr>
    <tr>
      <th>24</th>
      <td>The Good Doctor: Temporada 1: Ella</td>
      <td>7/9/21</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Valeria: Temporada 2: Reflejo</td>
      <td>7/9/21</td>
    </tr>
    <tr>
      <th>26</th>
      <td>La casa de papel: Parte 5: El espectáculo de l...</td>
      <td>6/9/21</td>
    </tr>
    <tr>
      <th>27</th>
      <td>The Good Doctor: Temporada 1: Siete razones</td>
      <td>6/9/21</td>
    </tr>
    <tr>
      <th>28</th>
      <td>La casa de papel: de Tokio a Berlín: Volumen 1...</td>
      <td>5/9/21</td>
    </tr>
    <tr>
      <th>29</th>
      <td>La casa de papel: Parte 5: ¿Crees en la reenca...</td>
      <td>4/9/21</td>
    </tr>
    <tr>
      <th>30</th>
      <td>The Good Doctor: Temporada 1: Islas: 2.ª parte</td>
      <td>3/9/21</td>
    </tr>
    <tr>
      <th>31</th>
      <td>La casa de papel: Parte 5: El final del camino</td>
      <td>3/9/21</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Valeria: Temporada 2: Titilar</td>
      <td>3/9/21</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Valeria: Temporada 2: Tortilla challenge</td>
      <td>3/9/21</td>
    </tr>
    <tr>
      <th>34</th>
      <td>El Reino vacío: Temporada 1: El archivo Osorio</td>
      <td>2/9/21</td>
    </tr>
    <tr>
      <th>35</th>
      <td>El Reino vacío: Temporada 1: Bastardos</td>
      <td>2/9/21</td>
    </tr>
    <tr>
      <th>36</th>
      <td>The Good Doctor: Temporada 1: Islas: 1.ª parte</td>
      <td>2/9/21</td>
    </tr>
    <tr>
      <th>37</th>
      <td>The Good Doctor: Temporada 1: Sacrificio</td>
      <td>2/9/21</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Valeria: Temporada 2: Fundición</td>
      <td>2/9/21</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Valeria: Temporada 2: Expuesta</td>
      <td>2/9/21</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Valeria: Temporada 2: Un libro no es un libro</td>
      <td>2/9/21</td>
    </tr>
    <tr>
      <th>41</th>
      <td>The Good Doctor: Temporada 1: Intangibles</td>
      <td>1/9/21</td>
    </tr>
    <tr>
      <th>42</th>
      <td>The Good Doctor: Temporada 1: Manzana</td>
      <td>31/8/21</td>
    </tr>
    <tr>
      <th>43</th>
      <td>The Good Doctor: Temporada 1: 22 pasos</td>
      <td>31/8/21</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Valeria: Temporada 2: Si no sabes qué hacer, e...</td>
      <td>31/8/21</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Valeria: Temporada 2: Dejar de huir</td>
      <td>31/8/21</td>
    </tr>
    <tr>
      <th>46</th>
      <td>The Good Doctor: Temporada 1: De mentira no</td>
      <td>30/8/21</td>
    </tr>
    <tr>
      <th>47</th>
      <td>The Good Doctor: Temporada 1: Uno coma tres po...</td>
      <td>30/8/21</td>
    </tr>
    <tr>
      <th>48</th>
      <td>El Reino vacío: Temporada 1: El círculo de baba</td>
      <td>30/8/21</td>
    </tr>
    <tr>
      <th>49</th>
      <td>El Reino vacío: Temporada 1: Expiación</td>
      <td>28/8/21</td>
    </tr>
  </tbody>
</table>
</div>



Vemos que parece que existe un patrón en el que diferentes partes se se separan mediante dos puntos. Vamos a contar en cuantas partes distintas se seperan nuestros contenidos


```python
separacion_lista = df.Title.str.split(pat = ':', expand=False).to_frame()
separacion_lista
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[The Good Doctor,  Temporada 1,  Sonrisa]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[The Good Doctor,  Temporada 1,  Dolor]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>1955</th>
      <td>[21,  Blackjack]</td>
    </tr>
    <tr>
      <th>1956</th>
      <td>[Living on One Dollar]</td>
    </tr>
    <tr>
      <th>1957</th>
      <td>[Descifrando enigma]</td>
    </tr>
    <tr>
      <th>1958</th>
      <td>[Madres forzosas,  Temporada 1,  Día de mudanza]</td>
    </tr>
    <tr>
      <th>1959</th>
      <td>[Madres forzosas,  Temporada 1,  Vuelta al pri...</td>
    </tr>
  </tbody>
</table>
<p>1960 rows × 1 columns</p>
</div>




```python
separacion_lista['num_partes'] = separacion_lista.Title.apply(len)
separacion_lista
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>num_partes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[The Good Doctor,  Temporada 1,  Sonrisa]</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[The Good Doctor,  Temporada 1,  Dolor]</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1955</th>
      <td>[21,  Blackjack]</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1956</th>
      <td>[Living on One Dollar]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1957</th>
      <td>[Descifrando enigma]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1958</th>
      <td>[Madres forzosas,  Temporada 1,  Día de mudanza]</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1959</th>
      <td>[Madres forzosas,  Temporada 1,  Vuelta al pri...</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>1960 rows × 2 columns</p>
</div>




```python
separacion_lista.num_partes.value_counts()
```




    3    1584
    4     173
    1     145
    5      30
    2      28
    Name: num_partes, dtype: int64



Vamos a analizar los títulos por número de partes a ver si hay algún patrón.


```python
separacion_lista.loc[separacion_lista.num_partes == 1].head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>num_partes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>53</th>
      <td>[La odisea de los giles]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>82</th>
      <td>[Regreso al futuro]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>83</th>
      <td>[Regreso al futuro 2]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>91</th>
      <td>[I Am Mother]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>226</th>
      <td>[Sin rodeos]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>245</th>
      <td>[Bronx]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>256</th>
      <td>[Tigre Blanco]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>294</th>
      <td>[Noticias del gran mundo]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>316</th>
      <td>[Bajocero]</td>
      <td>1</td>
    </tr>
    <tr>
      <th>317</th>
      <td>[Ya no estoy aquí]</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
separacion_lista.loc[separacion_lista.num_partes == 2].head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>num_partes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>152</th>
      <td>[Men in Black,  International]</td>
      <td>2</td>
    </tr>
    <tr>
      <th>516</th>
      <td>[Work It,  Al ritmo de los sueños]</td>
      <td>2</td>
    </tr>
    <tr>
      <th>601</th>
      <td>[Se lo llevaron,  recuerdos de una niña de Cam...</td>
      <td>2</td>
    </tr>
    <tr>
      <th>619</th>
      <td>[Oprah Winfrey presenta,  Así nos ven ahora]</td>
      <td>2</td>
    </tr>
    <tr>
      <th>679</th>
      <td>[La casa de papel,  El fenómeno]</td>
      <td>2</td>
    </tr>
    <tr>
      <th>743</th>
      <td>[Warcraft,  El origen]</td>
      <td>2</td>
    </tr>
    <tr>
      <th>832</th>
      <td>[Harry Potter y las Reliquias de la Muerte,  P...</td>
      <td>2</td>
    </tr>
    <tr>
      <th>833</th>
      <td>[Harry Potter y las Reliquias de la Muerte,  P...</td>
      <td>2</td>
    </tr>
    <tr>
      <th>879</th>
      <td>[El Camino,  Una película de Breaking Bad]</td>
      <td>2</td>
    </tr>
    <tr>
      <th>953</th>
      <td>[Gaga,  Five Foot Two]</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
separacion_lista.loc[separacion_lista.num_partes == 3].head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>num_partes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[The Good Doctor,  Temporada 1,  Sonrisa]</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[The Good Doctor,  Temporada 1,  Dolor]</td>
      <td>3</td>
    </tr>
    <tr>
      <th>19</th>
      <td>[La casa de papel,  Parte 5,  Vivir muchas vidas]</td>
      <td>3</td>
    </tr>
    <tr>
      <th>20</th>
      <td>[Al borde,  Temporada 1,  Hace unos dos meses]</td>
      <td>3</td>
    </tr>
    <tr>
      <th>21</th>
      <td>[Bolívar,  Temporada 1,  Episodio 1]</td>
      <td>3</td>
    </tr>
    <tr>
      <th>22</th>
      <td>[La casa de papel,  Parte 5,  Tu sitio en el c...</td>
      <td>3</td>
    </tr>
    <tr>
      <th>23</th>
      <td>[The Good Doctor,  Temporada 1,  De corazón]</td>
      <td>3</td>
    </tr>
    <tr>
      <th>24</th>
      <td>[The Good Doctor,  Temporada 1,  Ella]</td>
      <td>3</td>
    </tr>
    <tr>
      <th>25</th>
      <td>[Valeria,  Temporada 2,  Reflejo]</td>
      <td>3</td>
    </tr>
    <tr>
      <th>26</th>
      <td>[La casa de papel,  Parte 5,  El espectáculo d...</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
separacion_lista.loc[separacion_lista.num_partes == 4].head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>num_partes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>28</th>
      <td>[La casa de papel,  de Tokio a Berlín,  Volume...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>30</th>
      <td>[The Good Doctor,  Temporada 1,  Islas,  2.ª p...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>36</th>
      <td>[The Good Doctor,  Temporada 1,  Islas,  1.ª p...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>64</th>
      <td>[Master of None,  Temporada 3,  Momentos de am...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>65</th>
      <td>[Master of None,  Temporada 3,  Momentos de am...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>66</th>
      <td>[Master of None,  Temporada 3,  Momentos de am...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>77</th>
      <td>[Master of None,  Temporada 3,  Momentos de am...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>79</th>
      <td>[Master of None,  Temporada 3,  Momentos de am...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>127</th>
      <td>[Élite historias breves,  Carla Samuel,  Tempo...</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>




```python
separacion_lista.loc[separacion_lista.num_partes == 5].head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>num_partes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>7</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>8</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>9</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>10</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>11</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



## 3. CREACIÓN DE VARIABLES

Nuestro dataset original tiene muy pocas variables, pero con un poco de trabajo podemos generar nuevas variables. Este proceso se llama "feature extraction".

### 3.1 VARIABLES DERIVADAS DEL TÍTULO


```python
import numpy as np

separacion_lista['tipo'] = np.where(separacion_lista.num_partes <3, 'pelicula', 'serie')
separacion_lista
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>num_partes</th>
      <th>tipo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>[The Good Doctor,  Temporada 1,  Sonrisa]</td>
      <td>3</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>1</th>
      <td>[The Good Doctor,  Temporada 1,  Dolor]</td>
      <td>3</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>2</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>4</th>
      <td>[BattleBots,  Peleas de robots,  Temporada 2, ...</td>
      <td>5</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1955</th>
      <td>[21,  Blackjack]</td>
      <td>2</td>
      <td>pelicula</td>
    </tr>
    <tr>
      <th>1956</th>
      <td>[Living on One Dollar]</td>
      <td>1</td>
      <td>pelicula</td>
    </tr>
    <tr>
      <th>1957</th>
      <td>[Descifrando enigma]</td>
      <td>1</td>
      <td>pelicula</td>
    </tr>
    <tr>
      <th>1958</th>
      <td>[Madres forzosas,  Temporada 1,  Día de mudanza]</td>
      <td>3</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>1959</th>
      <td>[Madres forzosas,  Temporada 1,  Vuelta al pri...</td>
      <td>3</td>
      <td>serie</td>
    </tr>
  </tbody>
</table>
<p>1960 rows × 3 columns</p>
</div>




```python
df = pd.concat([df,separacion_lista['tipo']], axis = 1)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>Date</th>
      <th>tipo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>The Good Doctor: Temporada 1: Sonrisa</td>
      <td>12/9/21</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Good Doctor: Temporada 1: Dolor</td>
      <td>12/9/21</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Un ...</td>
      <td>10/9/21</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Gra...</td>
      <td>10/9/21</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BattleBots: Peleas de robots: Temporada 2: La ...</td>
      <td>10/9/21</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1955</th>
      <td>21: Blackjack</td>
      <td>1/1/17</td>
      <td>pelicula</td>
    </tr>
    <tr>
      <th>1956</th>
      <td>Living on One Dollar</td>
      <td>28/12/16</td>
      <td>pelicula</td>
    </tr>
    <tr>
      <th>1957</th>
      <td>Descifrando enigma</td>
      <td>27/12/16</td>
      <td>pelicula</td>
    </tr>
    <tr>
      <th>1958</th>
      <td>Madres forzosas: Temporada 1: Día de mudanza</td>
      <td>21/12/16</td>
      <td>serie</td>
    </tr>
    <tr>
      <th>1959</th>
      <td>Madres forzosas: Temporada 1: Vuelta al princi...</td>
      <td>21/12/16</td>
      <td>serie</td>
    </tr>
  </tbody>
</table>
<p>1960 rows × 3 columns</p>
</div>



Vamos a dividir los títulos en sus diferentes niveles y generar así nuevas variables


```python
separacion_cols = df.Title.str.split(pat = ':', expand=True)
separacion_cols
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Sonrisa</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Dolor</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Un robot deslumbrante</td>
      <td>La final</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Grandes esperanzas</td>
      <td>Los cuartos de final</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>La rebelión de las máquinas</td>
      <td>Octavos de final, 2.ª parte</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1955</th>
      <td>21</td>
      <td>Blackjack</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1956</th>
      <td>Living on One Dollar</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1957</th>
      <td>Descifrando enigma</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1958</th>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Día de mudanza</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1959</th>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Vuelta al principio, de nuevo</td>
      <td>None</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
<p>1960 rows × 5 columns</p>
</div>




```python
separacion_cols.columns = ['nivel1','nivel2','nivel3','nivel4','nivel5']
separacion_cols
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>nivel1</th>
      <th>nivel2</th>
      <th>nivel3</th>
      <th>nivel4</th>
      <th>nivel5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Sonrisa</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Dolor</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Un robot deslumbrante</td>
      <td>La final</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Grandes esperanzas</td>
      <td>Los cuartos de final</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>La rebelión de las máquinas</td>
      <td>Octavos de final, 2.ª parte</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1955</th>
      <td>21</td>
      <td>Blackjack</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1956</th>
      <td>Living on One Dollar</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1957</th>
      <td>Descifrando enigma</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1958</th>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Día de mudanza</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1959</th>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Vuelta al principio, de nuevo</td>
      <td>None</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
<p>1960 rows × 5 columns</p>
</div>




```python
df = pd.concat([df,separacion_cols], axis = 1)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>Date</th>
      <th>tipo</th>
      <th>nivel1</th>
      <th>nivel2</th>
      <th>nivel3</th>
      <th>nivel4</th>
      <th>nivel5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>The Good Doctor: Temporada 1: Sonrisa</td>
      <td>12/9/21</td>
      <td>serie</td>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Sonrisa</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Good Doctor: Temporada 1: Dolor</td>
      <td>12/9/21</td>
      <td>serie</td>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Dolor</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Un ...</td>
      <td>10/9/21</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Un robot deslumbrante</td>
      <td>La final</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Gra...</td>
      <td>10/9/21</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Grandes esperanzas</td>
      <td>Los cuartos de final</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BattleBots: Peleas de robots: Temporada 2: La ...</td>
      <td>10/9/21</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>La rebelión de las máquinas</td>
      <td>Octavos de final, 2.ª parte</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1955</th>
      <td>21: Blackjack</td>
      <td>1/1/17</td>
      <td>pelicula</td>
      <td>21</td>
      <td>Blackjack</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1956</th>
      <td>Living on One Dollar</td>
      <td>28/12/16</td>
      <td>pelicula</td>
      <td>Living on One Dollar</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1957</th>
      <td>Descifrando enigma</td>
      <td>27/12/16</td>
      <td>pelicula</td>
      <td>Descifrando enigma</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1958</th>
      <td>Madres forzosas: Temporada 1: Día de mudanza</td>
      <td>21/12/16</td>
      <td>serie</td>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Día de mudanza</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>1959</th>
      <td>Madres forzosas: Temporada 1: Vuelta al princi...</td>
      <td>21/12/16</td>
      <td>serie</td>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Vuelta al principio, de nuevo</td>
      <td>None</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
<p>1960 rows × 8 columns</p>
</div>



### 3.2 VARIABLES DERIVADAS DE LA FECHA

Podemos extraer los diferentes compontentes de una fecha para generar nuevas variables.


```python
df['fecha'] = pd.to_datetime(df.Date)
df.drop(columns = 'Date',inplace = True)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>tipo</th>
      <th>nivel1</th>
      <th>nivel2</th>
      <th>nivel3</th>
      <th>nivel4</th>
      <th>nivel5</th>
      <th>fecha</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>The Good Doctor: Temporada 1: Sonrisa</td>
      <td>serie</td>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Sonrisa</td>
      <td>None</td>
      <td>None</td>
      <td>2021-12-09</td>
    </tr>
    <tr>
      <th>1</th>
      <td>The Good Doctor: Temporada 1: Dolor</td>
      <td>serie</td>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Dolor</td>
      <td>None</td>
      <td>None</td>
      <td>2021-12-09</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Un ...</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Un robot deslumbrante</td>
      <td>La final</td>
      <td>2021-10-09</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Gra...</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Grandes esperanzas</td>
      <td>Los cuartos de final</td>
      <td>2021-10-09</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BattleBots: Peleas de robots: Temporada 2: La ...</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>La rebelión de las máquinas</td>
      <td>Octavos de final, 2.ª parte</td>
      <td>2021-10-09</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1955</th>
      <td>21: Blackjack</td>
      <td>pelicula</td>
      <td>21</td>
      <td>Blackjack</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>2017-01-01</td>
    </tr>
    <tr>
      <th>1956</th>
      <td>Living on One Dollar</td>
      <td>pelicula</td>
      <td>Living on One Dollar</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>2016-12-28</td>
    </tr>
    <tr>
      <th>1957</th>
      <td>Descifrando enigma</td>
      <td>pelicula</td>
      <td>Descifrando enigma</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>2016-12-27</td>
    </tr>
    <tr>
      <th>1958</th>
      <td>Madres forzosas: Temporada 1: Día de mudanza</td>
      <td>serie</td>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Día de mudanza</td>
      <td>None</td>
      <td>None</td>
      <td>2016-12-21</td>
    </tr>
    <tr>
      <th>1959</th>
      <td>Madres forzosas: Temporada 1: Vuelta al princi...</td>
      <td>serie</td>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Vuelta al principio, de nuevo</td>
      <td>None</td>
      <td>None</td>
      <td>2016-12-21</td>
    </tr>
  </tbody>
</table>
<p>1960 rows × 8 columns</p>
</div>




```python
df.set_index('fecha', inplace = True)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>tipo</th>
      <th>nivel1</th>
      <th>nivel2</th>
      <th>nivel3</th>
      <th>nivel4</th>
      <th>nivel5</th>
    </tr>
    <tr>
      <th>fecha</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-12-09</th>
      <td>The Good Doctor: Temporada 1: Sonrisa</td>
      <td>serie</td>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Sonrisa</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2021-12-09</th>
      <td>The Good Doctor: Temporada 1: Dolor</td>
      <td>serie</td>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Dolor</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2021-10-09</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Un ...</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Un robot deslumbrante</td>
      <td>La final</td>
    </tr>
    <tr>
      <th>2021-10-09</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Gra...</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Grandes esperanzas</td>
      <td>Los cuartos de final</td>
    </tr>
    <tr>
      <th>2021-10-09</th>
      <td>BattleBots: Peleas de robots: Temporada 2: La ...</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>La rebelión de las máquinas</td>
      <td>Octavos de final, 2.ª parte</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2017-01-01</th>
      <td>21: Blackjack</td>
      <td>pelicula</td>
      <td>21</td>
      <td>Blackjack</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2016-12-28</th>
      <td>Living on One Dollar</td>
      <td>pelicula</td>
      <td>Living on One Dollar</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2016-12-27</th>
      <td>Descifrando enigma</td>
      <td>pelicula</td>
      <td>Descifrando enigma</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2016-12-21</th>
      <td>Madres forzosas: Temporada 1: Día de mudanza</td>
      <td>serie</td>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Día de mudanza</td>
      <td>None</td>
      <td>None</td>
    </tr>
    <tr>
      <th>2016-12-21</th>
      <td>Madres forzosas: Temporada 1: Vuelta al princi...</td>
      <td>serie</td>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Vuelta al principio, de nuevo</td>
      <td>None</td>
      <td>None</td>
    </tr>
  </tbody>
</table>
<p>1960 rows × 7 columns</p>
</div>



Creamos una función para extraer los componentes.


```python
def atributos_fechas(data):
  data['año'] = data.index.year
  data['mes'] = data.index.month_name()
  data['dia_mes'] = data.index.day
  data['dia_semana'] = data.index.day_name()
  return(data)
```


```python
atributos_fechas(df)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>tipo</th>
      <th>nivel1</th>
      <th>nivel2</th>
      <th>nivel3</th>
      <th>nivel4</th>
      <th>nivel5</th>
      <th>año</th>
      <th>mes</th>
      <th>dia_mes</th>
      <th>dia_semana</th>
    </tr>
    <tr>
      <th>fecha</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-12-09</th>
      <td>The Good Doctor: Temporada 1: Sonrisa</td>
      <td>serie</td>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Sonrisa</td>
      <td>None</td>
      <td>None</td>
      <td>2021</td>
      <td>December</td>
      <td>9</td>
      <td>Thursday</td>
    </tr>
    <tr>
      <th>2021-12-09</th>
      <td>The Good Doctor: Temporada 1: Dolor</td>
      <td>serie</td>
      <td>The Good Doctor</td>
      <td>Temporada 1</td>
      <td>Dolor</td>
      <td>None</td>
      <td>None</td>
      <td>2021</td>
      <td>December</td>
      <td>9</td>
      <td>Thursday</td>
    </tr>
    <tr>
      <th>2021-10-09</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Un ...</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Un robot deslumbrante</td>
      <td>La final</td>
      <td>2021</td>
      <td>October</td>
      <td>9</td>
      <td>Saturday</td>
    </tr>
    <tr>
      <th>2021-10-09</th>
      <td>BattleBots: Peleas de robots: Temporada 2: Gra...</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>Grandes esperanzas</td>
      <td>Los cuartos de final</td>
      <td>2021</td>
      <td>October</td>
      <td>9</td>
      <td>Saturday</td>
    </tr>
    <tr>
      <th>2021-10-09</th>
      <td>BattleBots: Peleas de robots: Temporada 2: La ...</td>
      <td>serie</td>
      <td>BattleBots</td>
      <td>Peleas de robots</td>
      <td>Temporada 2</td>
      <td>La rebelión de las máquinas</td>
      <td>Octavos de final, 2.ª parte</td>
      <td>2021</td>
      <td>October</td>
      <td>9</td>
      <td>Saturday</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2017-01-01</th>
      <td>21: Blackjack</td>
      <td>pelicula</td>
      <td>21</td>
      <td>Blackjack</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>2017</td>
      <td>January</td>
      <td>1</td>
      <td>Sunday</td>
    </tr>
    <tr>
      <th>2016-12-28</th>
      <td>Living on One Dollar</td>
      <td>pelicula</td>
      <td>Living on One Dollar</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>2016</td>
      <td>December</td>
      <td>28</td>
      <td>Wednesday</td>
    </tr>
    <tr>
      <th>2016-12-27</th>
      <td>Descifrando enigma</td>
      <td>pelicula</td>
      <td>Descifrando enigma</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>2016</td>
      <td>December</td>
      <td>27</td>
      <td>Tuesday</td>
    </tr>
    <tr>
      <th>2016-12-21</th>
      <td>Madres forzosas: Temporada 1: Día de mudanza</td>
      <td>serie</td>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Día de mudanza</td>
      <td>None</td>
      <td>None</td>
      <td>2016</td>
      <td>December</td>
      <td>21</td>
      <td>Wednesday</td>
    </tr>
    <tr>
      <th>2016-12-21</th>
      <td>Madres forzosas: Temporada 1: Vuelta al princi...</td>
      <td>serie</td>
      <td>Madres forzosas</td>
      <td>Temporada 1</td>
      <td>Vuelta al principio, de nuevo</td>
      <td>None</td>
      <td>None</td>
      <td>2016</td>
      <td>December</td>
      <td>21</td>
      <td>Wednesday</td>
    </tr>
  </tbody>
</table>
<p>1960 rows × 11 columns</p>
</div>



## 4. ANALISIS

¿Cuanto tiempo hace que tengo contratado Netflix?


```python
from datetime import date

hoy = pd.Timestamp(date.today())

primer_dia = df.index.min()

tiempo = hoy - primer_dia

print(f'David, llevas usando Netflix {tiempo.days} dias')
```

    David, llevas usando Netflix 1727 dias


¿Cuanto me he gastado en Netflix hasta ahora?


```python
coste_mensual = 12

gasto = tiempo.days / 30 * coste_mensual

print(f'David, hasta ahora te has gastado {gasto} euros en Netflix')
```

    David, hasta ahora te has gastado 690.8000000000001 euros en Netflix


¿Cuanto tiempo de mi vida le dedico cada año a Netflix?


```python
media_min_serie = 45
media_min_peli = 100

consumo = df.loc[df.año < 2021].groupby('tipo').Title.count()

minutos_pelis_año = consumo['pelicula'] * media_min_peli / 4

minutos_series_año = consumo['serie'] * media_min_serie / 4

dias_pelis_año = minutos_pelis_año / 60 / 24

dias_series_año = minutos_series_año / 60 / 24

print(f'David, al año dedicas {round(dias_series_año)} dias de tu vida a ver series y {round(dias_pelis_año)} dias de tu vida a ver pelis')
```

    David, al año dedicas 11 dias de tu vida a ver series y 3 dias de tu vida a ver pelis


¿Cuales son las 10 series de las que he visto más capítulos?


```python
df.loc[df.tipo == 'serie'].nivel1.value_counts(ascending = True).tail(10).plot.barh(cmap = 'Pastel2');
```


![png](output_42_0.png)


Echo en falta mi serie favorita! Hijos de la Anarquía. Vamos a hacer una consulta de los títulos que incluyan ese nombre para ver por qué.


```python
df[df.Title.str.contains('Hijos')]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Title</th>
      <th>tipo</th>
      <th>nivel1</th>
      <th>nivel2</th>
      <th>nivel3</th>
      <th>nivel4</th>
      <th>nivel5</th>
      <th>año</th>
      <th>mes</th>
      <th>dia_mes</th>
      <th>dia_semana</th>
    </tr>
    <tr>
      <th>fecha</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2017-02-04</th>
      <td>Hijos de la anarquía: Temporada 7: Las buenas ...</td>
      <td>serie</td>
      <td>Hijos de la anarquía</td>
      <td>Temporada 7</td>
      <td>Las buenas obras de papá</td>
      <td>None</td>
      <td>None</td>
      <td>2017</td>
      <td>February</td>
      <td>4</td>
      <td>Saturday</td>
    </tr>
    <tr>
      <th>2017-12-03</th>
      <td>Hijos de la anarquía: Temporada 7: Rosa de sangre</td>
      <td>serie</td>
      <td>Hijos de la anarquía</td>
      <td>Temporada 7</td>
      <td>Rosa de sangre</td>
      <td>None</td>
      <td>None</td>
      <td>2017</td>
      <td>December</td>
      <td>3</td>
      <td>Sunday</td>
    </tr>
    <tr>
      <th>2017-12-03</th>
      <td>Hijos de la anarquía: Temporada 7: El gran lam...</td>
      <td>serie</td>
      <td>Hijos de la anarquía</td>
      <td>Temporada 7</td>
      <td>El gran lamento</td>
      <td>None</td>
      <td>None</td>
      <td>2017</td>
      <td>December</td>
      <td>3</td>
      <td>Sunday</td>
    </tr>
    <tr>
      <th>2017-05-03</th>
      <td>Hijos de la anarquía: Temporada 7: Fe y abatim...</td>
      <td>serie</td>
      <td>Hijos de la anarquía</td>
      <td>Temporada 7</td>
      <td>Fe y abatimiento</td>
      <td>None</td>
      <td>None</td>
      <td>2017</td>
      <td>May</td>
      <td>3</td>
      <td>Wednesday</td>
    </tr>
    <tr>
      <th>2017-02-18</th>
      <td>Hijos de la anarquía: Temporada 7: El hombre e...</td>
      <td>serie</td>
      <td>Hijos de la anarquía</td>
      <td>Temporada 7</td>
      <td>El hombre es de lo que no hay</td>
      <td>None</td>
      <td>None</td>
      <td>2017</td>
      <td>February</td>
      <td>18</td>
      <td>Saturday</td>
    </tr>
    <tr>
      <th>2017-12-02</th>
      <td>Hijos de la anarquía: Temporada 7: Separando c...</td>
      <td>serie</td>
      <td>Hijos de la anarquía</td>
      <td>Temporada 7</td>
      <td>Separando cuervos</td>
      <td>None</td>
      <td>None</td>
      <td>2017</td>
      <td>December</td>
      <td>2</td>
      <td>Saturday</td>
    </tr>
    <tr>
      <th>2017-10-02</th>
      <td>Hijos de la anarquía: Temporada 7: Mangas verdes</td>
      <td>serie</td>
      <td>Hijos de la anarquía</td>
      <td>Temporada 7</td>
      <td>Mangas verdes</td>
      <td>None</td>
      <td>None</td>
      <td>2017</td>
      <td>October</td>
      <td>2</td>
      <td>Monday</td>
    </tr>
    <tr>
      <th>2017-03-02</th>
      <td>Hijos de la anarquía: Temporada 7: Si los pill...</td>
      <td>serie</td>
      <td>Hijos de la anarquía</td>
      <td>Temporada 7</td>
      <td>Si los pillas, te los cargas</td>
      <td>None</td>
      <td>None</td>
      <td>2017</td>
      <td>March</td>
      <td>2</td>
      <td>Thursday</td>
    </tr>
  </tbody>
</table>
</div>



¿Qué día de la semana suelo ver más series?


```python
df.loc[df.tipo == 'serie', 'dia_semana'].value_counts().plot(kind = 'bar');
```


![png](output_46_0.png)


¿Existen diferencias en cuando veo Netflix entre series y películas?


```python
import seaborn as sns

sns.countplot(data = df, x = 'dia_semana', hue = 'tipo', palette= 'pastel');
```


![png](output_48_0.png)


¿El consumo a lo largo del año es constante o hay meses que consumo más?


```python
df.loc[df.año < 2021].mes.value_counts().plot.bar();
```


![png](output_50_0.png)


¿Afectó el confinamiento a la cantidad de uso que hice de Netflix?


```python
df.año.value_counts().plot.bar();
```


![png](output_52_0.png)

