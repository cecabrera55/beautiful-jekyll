---
layout: post
title: Plebiscito en Twitter
date: '2016-09-27'
permalink: /es/blog/twitter-plebiscito/
published: false
share-img: /img/posts/twitter-plebiscito/wordcloud.png
tags:
  - Español
  - Twitter
  - Plebliscito
  - Paz
  - Colombia
  - Business Intelligence
  - Web Scraping
  - R
---

> ###### Este blog fue publicado originalmente como una [nota de interés](http://www.ikonosoft.com/notas-de-interes/121-el-plebiscito-en-twitter) en [Ikonosoft](http://www.ikonosoft.com/). 

Colombia se encuentra en la recta final de la votación entre el Si y el No para el plebiscito. La guerra con las FARC ha marcado historia en la patria y aunque la paz soñada no depende de una firma, el país se manifiesta en redes sociales.

Twitter es una red social que comparte públicamente el contenido y la interacción entre sus usuarios. Es el ágora de nuestra sociedad virtual. Por lo tanto, analizar y visualizar métricas relevantes en su contenido podría dar una radiografía en tiempo real de lo que está sucediendo en torno a la “paz”: específicamente al plebiscito.

Se tomó la iniciativa de almacenar, analizar y visualizar datos de Twitter con el ánimo de descubrir cuál es el __sentimiento__ e __intención de voto__ en los comentarios que contuviesen la palabra “plebiscito”. Del total de tweets publicados el 20 de septiembre de 2016, se analizaron los sentimientos de 350 y la intención de voto de 97 para visualizar su contenido en una nube de palabras. 

# Análisis descriptivo

![Plebiscito en Twitter]({{ site.url }}/img/posts/twitter-plebiscito/Prop.jpg)

El 84% de las personas que demostraron una intencion de voto al No en el plebiscito manifestaron un sentimiento negativo al redactar el tweet. De manera opuesta, el 60% de los que respondieron Si, sostuvieron un sentimiento positivo en su comentario.

![Plebiscito en Twitter]({{ site.url }}/img/posts/twitter-plebiscito/Rplot.png)

De los 97 tweets analizados que demostraron una intención de voto, el 62% (60/97) dijo que Si al plebiscito en Colombia, mientras el 38% manifestó su inconformidad con el mismo.

Apesar que la cantidad de tweets analizados es relativamente pequeña, el resultado de esta muestra es coherente con las [principales encuestadoras del plebiscito](https://docs.google.com/spreadsheets/d/1qa8ijawTzu1ztXh5z605DRvc05qK4Db04jwnP8nkNrc/pubhtml?gid=1716392935&single=true):

* Invamer el 21 de septiembre: 68% votaría Si en el plebiscito.
* Datexco el 16 de septiembre: 59% votaría Si en el plebiscito.
* Ipsos Napoleón Franco el 10 de septiembre: 72% votaría Si.
* Centro Nacional de Consultoría el 7 de septiembre: 71%.

Del mismo modo, Andrés Gutiérrez ([linkedin](https://www.linkedin.com/profile/in/predictive?goback=) y [twitter](https://twitter.com/psirusteam)), director de evaluación del ICFES y PhD en Ciencias, comparte un resultado de 64.7% votando Si por el plebiscito a través de un [pronóstico basado en encuestas](http://hagutierrezro.blogspot.com.co/2016/09/forecasting-colombia-peace-plebiscite.html).

Independiente del resultado final, una muestra pequeña en Twitter ha logrado representar lo que las encuestadoras sugieren: el Si podría ser la opción ganadora en el plebiscito. 

# Nube de palabras

La Analítica de Texto (_Text Analytics_) es una de las ramas de la Inteligencia Artificial (AI). Antes de realizar modelos sofisticados de _Aprendizaje Automático_ (Machine Learning) o configurar un Sistema Distribuido de Archivos de Hadoop (HDFS) para el procesamiento en tiempo real de sentimientos (positivo, negativo, neutro) o emociones (feliz, triste, aburrido, con miedo, etc) es imprescindible configurar manualmente un set de datos inicial que sirva como _input_ para que los modelos "aprendan". Esta configuración se conoce como "Data Cleaning" o "Data Wraggling" y consiste en preparar los datos para analizarlos.

En este contexto, la "limpieza de datos" es eliminar de cada tweet las imágenes, videos, links, signos de puntuación y demás contenido que no sean palabras; además de los tweets repetidos (retweets). El resultado es una "bolsa de palabras" de la que se puede hacer una tabla de frecuencia. Las palabras más repetidas resaltarán más por aparecer más veces. La mejor forma de visualizarla es a través de una nube de palabras:

![Nube de palabras]({{ site.url }}/img/posts/twitter-plebiscito/wordcloud.png)

¿Para qué sirve? con la frecuencia con que aparecen las palabras es posible identificar intenciones implícitas en los tweets. La palabra “santos” aparece primordialmente en los tweets de personas que dicen No al plebiscito y deja la sensación que aquellos que no apoyan el plebiscito mencionan de manera negativa al Presidente Juan Manuel Santos pues al mismo tiempo resaltan las palabras “manipulada” y “engañarnos”. Las palabras “estable” y “duradera” hacen referencia a la pregunta principal del plebiscito: “¿Apoya usted el acuerdo final para la terminación del conflicto y la construcción de una paz estable y duradera?” y son usadas en tweets como: “Encuesta única, manipulada por Santos, para engañarnos a todos, de una manera estable y duradera” (referencia [aquí](https://twitter.com/GmoSaldarriaga/status/778230793869164544)) o “@VotaNOColombia plebiscito NO es para Paz estable y duradera , es solamente xa rechazar o aceptar acuerdo Timochenko-Santos #ColombiaconelNO” (referencia [aquí](https://twitter.com/penzador12/status/778232200768675840)).

Mientras las personas que expresan No enfocan sus intenciones en una persona a través de la palabra “santos”, las personas que votan Si mencionan más veces la palabra “colombia” asociándolo a una perspectiva plural o social. En el grupo de los que votan por el Si se resaltan hashtags (#) como “conargumentossi” y “sialapaz”; y menciones (@) como “franciscoderoux”. Este último sobresale pues el 19 de septiembre a las 10 de la noche (día anterior a la recolecta de los tweets), Revista Semana publicó un [artículo](http://www.semana.com/nacion/articulo/plebiscito-por-la-paz-francisco-de-roux-explica-su-postura/494351) sobre Francisco De Roux titulado “Los colombianos estamos enredados en un problema humano muy profundo” en donde el sacerdote manifiesta su intención de voto al Si.

# Conclusiones

Por razones confidenciales no se comparte el código fuente ni otras estadísticas. El lenguaje de programación fue R y con el total de tweets analizados se desarrolló un modelo de aprendizaje automático (machine learning) para estimar la _probabilidad que la intención de voto de un nuevo tweet fuese Si_. Dicho modelo usó el 80% de los datos para "entrenarse" y el 20% restante para "testear" su precisión.

Independiente del resultado del plebiscito, la ciencia de datos aporta valor a la gran cantidad de información disponible en internet a través de herramientas teóricas (como modelos estadísticos, métricas y tablas de frecuencias) y tecnológicas (como R, Python y paquetes de visualización de datos).