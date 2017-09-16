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

> ###### Basado en la [nota de interés](http://www.ikonosoft.com/notas-de-interes/121-el-plebiscito-en-twitter) de [Ikonosoft](http://www.ikonosoft.com/).

Colombia se encuentra en la recta final de la votación entre el Si y el No para el plebiscito. La guerra con las FARC ha marcado historia en la patria y aunque la paz soñada no depende de una firma, el país se manifiesta a través de las redes sociales frente a la situación.

Twitter es una red social que comparte públicamente el contenido y la interacción entre sus usuarios. Es el ágora de nuestra sociedad virtual. Por lo tanto, analizar y visualizar métricas relevantes en su contenido podría dar una radiografía en tiempo real de lo que está sucediendo en torno a la “paz”: específicamente al plebiscito.

En Ikognitive – unidad de negocio de Ikonosoft SAS especializada en la analítica e inteligencia de negocios – tomamos la iniciativa de almacenar, analizar y visualizar datos de Twitter con el ánimo de descubrir cuál es el sentimiento e intención de voto en los comentarios que contuviesen la palabra “plebiscito”. Analizamos los sentimientos de 350 tweets y la intención de voto de 97 del día 20 de septiembre y visualizamos su contenido a través de una nube de palabras.

# Análisis descriptivo

![Plebiscito en Twitter]({{ site.url }}/img/posts/twitter-plebiscito/Prop.jpg)

El 84% de las personas que demostraron una intencion de voto al No en el plebiscito manifestaron un sentimiento negativo al redactar el tweet. De manera opuesta, el 60% de los que respondieron Si, manifestaron un sentimiento positivo en su comentario.

![Plebiscito en Twitter]({{ site.url }}/img/posts/twitter-plebiscito/Rplot.png)

De los 97 tweets analizados que demostraron una intención de voto, el 62% (60/97) dijo que Si al plebiscito en Colombia, mientras el 38% manifestó su inconformidad con el mismo.

Apesar que la cantidad de tweets analizados es relativamente pequeña, el resultado de esta muestra es coherente con las [principales encuestadoras del plebiscito](https://docs.google.com/spreadsheets/d/1qa8ijawTzu1ztXh5z605DRvc05qK4Db04jwnP8nkNrc/pubhtml?gid=1716392935&single=true):

* Invamer el 21 de septiembre: 68% votaría Si en el plebiscito.
* Datexco el 16 de septiembre: 59% votaría Si en el plebiscito.
* Ipsos Napoleón Franco el 10 de septiembre: 72% votaría Si.
* Centro Nacional de Consultoría el 7 de septiembre: 71%.

Del mismo modo, Andrés Gutiérrez ([linkedin](https://www.linkedin.com/profile/in/predictive?goback=) y [twitter](https://twitter.com/psirusteam)), director de evaluación del ICFES y PhD en Ciencias, comparte un resultado de 64.7% votando Si por el plebiscito a través de un [pronóstico basado en encuestas](http://hagutierrezro.blogspot.com.co/2016/09/forecasting-colombia-peace-plebiscite.html).

# Nube de palabras

Para analizar texto en Twitter, es necesario llevar a cabo lo que en ciencia de datos se denomina como “limpieza de datos” (data cleaning). Esto es, eliminar las imágenes, videos, links, signos de puntuación y demás contenido irrelevante para el análisis; además de los tweets repetidos (retweets).

Luego, es posible visualizar la frecuencia con la que las palabras aparecen en Twitter:

![Nube de palabras]({{ site.url }}/img/posts/twitter-plebiscito/wordcloud.png)

¿Para qué sirve una nube de palabras? con la frecuencia con que aparecen las palabras es posible identificar intenciones implícitas en los tweets. La palabra “santos” aparece primordialmente en los tweets de personas que dicen No al plebiscito y deja la sensación que aquellos que no apoyan el plebiscito mencionan de manera negativa al Presidente Juan Manuel Santos pues al mismo tiempo resaltan las palabras “manipulada” y “engañarnos”. Las palabras “estable” y “duradera” hacen referencia a la pregunta principal del plebiscito: “¿Apoya usted el acuerdo final para la terminación del conflicto y la construcción de una paz estable y duradera?” y son usadas en tweets como: “Encuesta única, manipulada por Santos, para engañarnos a todos, de una manera estable y duradera” (referencia [aquí](https://twitter.com/GmoSaldarriaga/status/778230793869164544)) o “@VotaNOColombia plebiscito NO es para Paz estable y duradera , es solamente xa rechazar o aceptar acuerdo Timochenko-Santos #ColombiaconelNO” (referencia [aquí](https://twitter.com/penzador12/status/778232200768675840)).

Mientras las personas que manifiestan No enfocan sus intenciones en una persona a través de la palabra “santos”, las personas que votan Si mencionan más veces la palabra “colombia” asociándolo a una perspectiva social en el análisis. En el grupo de los que votan por el Si se resaltan hashtags (#) como “conargumentossi” y “sialapaz”; y menciones (@) como “franciscoderoux”. Este último sobresale pues el 19 de septiembre a las 10 de la noche, Revista Semana publicó un [artículo](http://www.semana.com/nacion/articulo/plebiscito-por-la-paz-francisco-de-roux-explica-su-postura/494351) sobre Francisco De Roux titulado “Los colombianos estamos enredados en un problema humano muy profundo” en donde el sacerdote manifiesta su intención de voto al Si.

# Conclusiones

Analizar las redes sociales es una de las fortalezas de Ikognitive. A partir del análisis en este blog, se realizó un modelo de aprendizaje de máquina (machine learning) para estimar la probabilidad que la intención de voto de un nuevo tweet sea Si o No. ¿Te interesa la analitica y quieres conocer el modelo? ¿Eres un pequeño negocio que necesita la inteligencia de negocios para tomar decisiones basadas en datos en tiempo real? ¿Una gran empresa buscando posibilidades de moverse a otros mercados? Contáctanos.

El objetivo de este blog es informar sobre las posibilidades que tiene la analítica en redes sociales y no pretende mostrar inclinación alguna sobre la intención de voto de los miembros de Ikognitive o de Ikonosoft SAS.
