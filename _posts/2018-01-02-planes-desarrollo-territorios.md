---
title: "Clasificación de Planes de Desarrollo"
layout: post
date: '2018-01-02'
published: yes
share-img: /img/blog/planesdnp/imagen7.png
tags:
- Español
- DNP
- Plan de Desarrollo
- Analítica
- Clustering
- Text Mining
- Correlation
- LDA
- Data Cleaning
- Machine Learning
permalink: /blog/planesdnp/
---

Los Planes de Desarrollo Territorial (PDT) son los documentos que dan la hoja de ruta de cada Departamento de Colombia durante los 4 años de gobierno electo. Los documentos con sus contenidos textuales están disponibles [aquí](/files/blog/pdt_dnp/data.zip). Putumayo es el único [plan de desarrollo](https://www.putumayo.gov.co/images/documentos/planes_y_programas/ordeN_726_16.pdf) faltante en esta base de datos.

Se analizarán los siguientes puntos:

1. Análisis de temas principales en cada PDT.
2. Clasificar los planes de acuerdo con algún criterio de similitud.
3. ¿Qué tipo de información reportaría a los evaluadores con el fin de hacer su tarea más
sencilla?

# Tabla de Contenidos

- [Preprocesamiento](#preprocesamiento)
  - [Tokenización](#tokenizacion)
  - [Filtrado](#filtrado)
  - [Lematización](#lematizacion)
  - [Stemming](#stemming)

- [Análisis Univariado](#univariado)
  - [Modelo de Vectores Espaciales](#espaciales)
  
- [Análisis Multivariado](#multivariado)
  - [Algoritmos de similaridad](#similaridad)
    - [Correlación](#correlacion)

- [Clasificación/Conglomerados](#conglomerados)
    - [Asignación de Dirichlet Latente](#dirichlet)

- [Probabilidades documento-tópico](#topico)
  - [Tópico 1: Proyecto de salud](#salud)
  - [Tópico 2: Educación y Cultura](#educacion)
- [Apuntes](#apuntes)
- [Recomendaciones](#recomendaciones)
- [Referencias](#referencias)


Analizar tal cantidad de datos (31 documentos y 59004 palabras) simultáneamente es una labor posible gracias a la [minería de texto](https://es.wikipedia.org/wiki/Miner%C3%ADa_de_textos). El objetivo de este blog es organizar, clasificar y visualizar los datos utilizando el lenguaje de programación [R](https://www.r-project.org/). El código fuente de este documento está alojado en [Github](https://github.com/cecabrera/pdt_dnp).

Los paquetes de R necesarios son:

```
require(shiny)
require(data.table)
require(tm)
require(ggplot2)
require(corrplot)
require(tidyr)
require(dplyr)
require(tidytext)
require(topicmodels)
```

Para comenzar, se almacena las rutas de los archivos de texto en una variable llamada `paths`. Esta lista será la ruta para cargar cada Plan de Desarrollo. Los nombres de cada Departamento se almacenarán en la variable `departments`. 

```
paths <- list.files(c("data"), pattern="\\.(TXT|txt)$", recursive = TRUE, full.names = TRUE)
departments <- sub(pattern = ".txt.*", "" , sub(pattern = ".*/", "", paths))
print(head(paths)) # Visualizar los primeros 6 elementos de la variable `paths`
```

<div>
  <a href="http://camicabrera.com/img/blog/planesdnp/datos1.png">
    <img src="http://camicabrera.com/img/blog/planesdnp/datos1.png" alt="">
  </a>
</div>

## Preprocesamiento {#preprocesamiento}

El preprocesamiento es un componente clave en muchos algoritmos de minado de texto y usualmente se resume en tareas como tokenización, filtrado, lematización y _stemming_ (o raíz de las palabras)[^1].

Se cargan los 31 documentos en una sola lista llamada `d`. Cada elemento de la lista representará un Departamento y contendrá las palabras de su respectivo PDT como una _tabla de datos_ de una columna. Se incrusta a la tabla una columna con `1` para denotar que la palabra aparece una vez y sirva luego para contar la frecuencia con la que aparece cada palabra. 

```
d <- lapply(paths, function(x){
  bag = data.table(names(fread(input = x, encoding = "UTF-8")))
  setnames(x = bag, old = "V1", new = sub(pattern = ".txt.*", "" , sub(pattern = ".*/", "", x)))
  bag[, f := 1]
  bag
})
names(d) <- departments
```

### Tokenización {#tokenizacion}

La tokenización consiste en separar las secuencias de caracteres en pedazos (palabras/frases) llamados tokens y, en el camino, desechar caracteres como puntuaciones. Nuestros documentos ya carecen de puntuaciones y están separados en su mayoría por palabras. 

### Filtrado {#filtrado}

Un filtro común es remover las _stop words_ o palabras que frecuentemente aparecen en el texto y que no aportan información: preposiciones y conjunciones, por ejemplo. 

En R, el paquete `tm` brinda una lista de 308 `stop-words` del español. Se filtrarán estas palabras, así como palabras que aparecen con mucha frecuencia y que son irrelevantes: "ee", "nn", "oo", "aa", etc.

A continuación las primeras 50. 

```
stop_es <- c(stopwords(kind = "es"), "ee", "nn", "oo", "aa", "aaa", "dd", "rr", "cc", "pp", "tt", "yy", "zz", "xx", "aafront", "aai", "aati", "aatis", "ddee", "aaí")
print(matrix(stop_es[1:50], nrow = 10, byrow = TRUE))
```
<div>
  <a href="http://camicabrera.com/img/blog/planesdnp/datos2.png">
    <img src="http://camicabrera.com/img/blog/planesdnp/datos2.png" alt="">
  </a>
</div>

Las suprimimos de todos los documentos:

```
d <- lapply(d, function(x){
  x[!x[[1]] %in% stop_es]
})
```

### Lematización {#lematizacion}

La lematización considera el análisis morfológico de las palabras: agrupar las diversas formas de una palabra de manera que puedan ser analizadas como un solo término. Los métodos de lematización intentan mapear verbos a su sentido infinitivo y los sustantivos a su forma singular. Para lematizar documentos, es necesario determinar para cada palabra si es un verbo, adjetivo o sustantivo. Dado que este proceso es tedioso y sujeto a errores, en la práctica se prefieren los métodos de _stemming_. 

### Stemming {#stemming}

Los métodos de _stemming_ le apuntan a encontrar la raíz de las palabras y usarla como método agrupador de palabras. Como es de esperarse, los algoritmos de _stemming_ dependen del lenguaje. 

Utilizando el algoritmo de _stemming_ de Porter y tomando como ejemplo las 30 palabras más mencionadas del Plan de Desarrollo del Huila encontramos las siguientes comparaciones. 


```
print(d$HUILA[, .N, keyby = HUILA][order(N, decreasing = T)][1:30,.(HUILA, stemDocument(x = HUILA,  language = "es"), N)])
```
<div>
  <a href="http://camicabrera.com/img/blog/planesdnp/datos3.png">
    <img src="http://camicabrera.com/img/blog/planesdnp/datos3.png" alt="">
  </a>
</div>

La búsqueda del origen de las palabras _(stemming)_ es un procedimiento delicado. En el español, remover las tildes en las palabras cambia el acento y puede entregar una connotación distinta a la palabra de origen: no es lo mismo "hábito" que "habito" que "habitó". 

A su vez, las raíces agrupan palabras de contexto diferentes: "partido", "parte", "partida" y "partes" conllevan a la misma raíz "part". Para efectos de este proyecto, se utilizarán las raíces de las palabras dejando a consideración del lector las implicaciones que esto tiene. 

```
d <- lapply(d, function(x){
  colname <- colnames(x)[1]
  x[[colname]] <- stemDocument(x[[colname]], language = "es")
  x
})
```

# Análisis Univariado {#univariado}

## Modelo de Vectores Espaciales {#espaciales}

Se definirá el _vocabulario_ como el conjunto que contiene todas las palabras presentes en todos los documentos. 

La manera más común de representar a todos los documentos es convirtiéndolos en vectores numéricos. A esta representación se le llama "Modelo de Vectores Espaciales" (VSM) y es utilizada en algoritmos de minado de texto y sistemas de recolección de información; al tiempo que facilita análisis eficientes de grandes colecciones de documentos[^2].

Colocamos todos los tokens en una misma matriz, se suman las frecuencias de cada palabra en cada documento y se ordenan en orden decreciente.  

```
d <- readRDS("chunks/stemm.RDS")
d <- lapply(d, function(x){
  name <- colnames(x)[1]
  x[, depart := name]
  setnames(x, old = name, new = "word")
  x
})
d <- rbindlist(d, use.names = T)[, sum(f), keyby = .(depart, word)]
print(d[order(V1, decreasing = T)])
```
<div>
  <a>
    <img src="http://camicabrera.com/img/blog/planesdnp/datos4.png" alt="">
  </a>
</div>

Filtra por los departamentos. Cada departamento resalta las 15 palabras (raíces) más mencionadas en cada Plan de Desarrollo. 

<div style="text-align:center;">
  <a>
    <img src="http://camicabrera.com/img/blog/planesdnp/imagen1.png" alt="">
    <img src="http://camicabrera.com/img/blog/planesdnp/imagen2.png" alt="">
    <img src="http://camicabrera.com/img/blog/planesdnp/imagen3.png" alt="">
  </a>
</div>

En VSM cada palabra es representada por un valor numérico que indica el peso (importancia) de la palabra en el documento. Se utilizará el modelo de _Term frequency-inverse document frequency_ (TF-IDF) para ponderar. Se moldea la tabla en una matriz de frecuencia:

```
f <- dcast.data.table(d, formula = "word ~ depart", value.var = "V1", fill = 0)
str(f)
```
<div>
  <a>
    <img src="http://camicabrera.com/img/blog/planesdnp/datos5.png" alt="">
  </a>
</div>

La variable `f` contiene la frecuencia con la que aparece cada palabra para cada departamento. Se calcula el término de frecuencia `TF-IDF`:

```
tf <- f[,-1,with=FALSE]
idf <- log((ncol(f)-1)/rowSums(tf))
q <- tf*idf
str(q)
q_melted <- melt.data.table(cbind(word = f$word, q), id.vars = "word", variable.name = "depart", value.name = "V1")
```

La variable `q` contiene la frecuencia ponderada por el TF-IDF de cada palabra. La variable `q_melted` es la misma variable `q` reducida a 3 columnas. A continuación las palabras más relevantes de cada departamento luego de ponderar por TF-IDF.

<div style="text-align:center;">
  <a>
    <img src="http://camicabrera.com/img/blog/planesdnp/imagen4.png" alt="">
    <img src="http://camicabrera.com/img/blog/planesdnp/imagen5.png" alt="">
    <img src="http://camicabrera.com/img/blog/planesdnp/imagen6.png" alt="">
  </a>
</div>

En los documentos está el reto de lidiar con las palabras pegadas. Por ejemplo, en Casanare hay palabras como "culturaprogram", "comunitarioprogram", "deportivosector" y "firmesector". En Arauca sucede algo similar: "propiosotr" haciendo alución a "propios otros" y "físicasecret" haciendo alución a "física secreto". Estas inconsistencias podrían comprometer la calidad de análisis entre documentos. 

En el Meta está de primero la palabra "guaro". Indagando en el documento se encontró que hay un municipio llamado "San Carlos de Guaroa" y el algoritmo luego de hacer el _stemm_ suprimió la "a" al final de "Guaroa". Este municipio tiene relevancia por las intenciones del departamento en aportar a sus necesidades sociales y económicas; nada que ver con el aguardiente. 

# Análisis Multivariado {#multivariado}

## Algoritmos de similaridad {#similaridad}

Las [4 métricas](https://stats.stackexchange.com/questions/289400/quantify-the-similarity-of-bags-of-words) de similaridad más utilidades en la literatura son[^3]:

* Jaccard Similarity
* Cosine Similarity
* Spearman's rank correlation
* Pearson chi2 test-based

Según [Kilgarriff (1997)](http://www.aclweb.org/anthology/W/W97/W97-0122.pdf), el test ideal para calcular la similaridad entre dos documentos es el _Pearson chi2 test-based_. Sin embargo, el poder computacional y concepto teórico es significativamente superior a utilizar _Cosine Similarity_ y entregan resultados relativamente similares. El _Cosine Similarity_ ha demostrado en otros experimentos ser mejor que el _Spearman's rank correlation_; aún cuando es una alternativa viable para calcular la similaridad entre documentos. La _Jaccard Similarity_ es la más simple de las similaridades y no tiene en consideración la frecuencia con la que aparecen las palabras. Por esta razón, la similaridad a trabajar será el _Cosine Similarity_.

### Correlación {#correlacion}

Cada Plan de Desarrollo se puede representar como un __vector__ de palabras. El coseno entre dos vectores determina cuán cercanos (igual a 1) o alejados (igual a -1) están en el plano n-dimensional. El cálculo del coseno es considerado también como el [_coeficiente de correlación no centrado_](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient) entre dos vectores y matemáticamente es similar al coeficiente de correlación de Pearson[^4]. Por eso se usará este como medida de la similaridad. 

```
corrplot(corr = cor(q, method = "pearson"), type = "lower", method = "pie", diag = TRUE, order = "FPC")
```

<div style="text-align:center;">
  <a>
    <img src="http://camicabrera.com/img/blog/planesdnp/imagen7.png" alt="">
  </a>
</div>

La matriz de correlación se ordenó por compomentes principales y por eso aparecen primero los departamentos con mayor correlación y de último los de menos. Se resalta los siguientes hallazgos:

* Quindío y Cundinamarca tienen la mayor similitud (0.903) de palabras en los Planes de Desarrollo. 
* Casanare es el departamento con mayor diferencias respecto a los demás departamentos. Esto puede deberse a las uniones de palabras en una sola o a diferencias reales en el contenido del documento. 
* [En Octubre de 2016](http://www.portafolio.co/economia/gobierno/dnp-destaco-los-planes-de-desarrollo-departamentales-501057), el DNP entregó un reconocimiento a las gobernaciones con desarrollo robusto, intermedio y temprano a Antioquia, Nariño y Caquetá, respectivamente[^5]. En el correlograma, estas tres aparecen seguidas demostrando esta aparente similitud. 

## Clasificación/Conglomerados {#conglomerados}

Si cada documento hiciese parte de una categoría _a priori_, se utilizarían algoritmos como:

1. Clasificadores del vecino más cercano
2. Clasificadores de árboles de decisión
3. Máquinas de Vectores de Soporte
4. Clasificador Naive Bayes

Un caso podría ser categorizar los documentos por región del país, tomar los Planes de Desarrollo Municipales y desarrollar un algoritmo de aprendizaje automático _(machine learning)_ para clasificar si las palabras usadas en los Municipios están correlacionadas (son similares) a las palabras usandas en los Departamentos. 

En vista que la clasificación se hará _a posteriori_ (no hay categorías predeterminadas), se utilizarán algoritmos de _clustering_ o conglomerados. En Minería de Texto algunos populares son:

1. Algortimos de Conglomerados Herárquicos. 
2. Conglomerado k-medias
3. Conglomerados probabilísticos y Modelos de Tópicos

Los algoritmos de conglomerados herárquicos están basados en distancias respecto a cada vector. Utilizan una función de similaridad para medir cuán cercano está un documento a otro. Es posible contruirlos de arriba a abajo (colocando cada documento en un grupo) o de abajo a arriba (colocando todos los documentos en un grupo). Los algoritmos de k-medias ubican un centroide por cada grupo y conglomera cada documento respecto a su cercanía a un centroide. Es ampliamente utilizado en problemas de datos estructurados. El cálculo del número óptimo de grupos (k) es considerado _NP-Hard_ dados los requerimientos computacionales requeridos para calcularlo; razón por la que existen heurísticas para hallar un óptimo local. 

La mayor implicación en los algoritmos anteriores es que a cada "observación" (en nuestro caso "palabra") es asignado a una única categoría. Sin embargo, como sucede en el lenguaje natural, existen palabras que podrían hacer parte de varias categorías (o tópicos/temas). Por eso, el modelaje de tópicos será el escogido para clasificar los Planes de Desarrollo Departamentales. El modelaje de tópicos es un método de clasificación no-supervisada utilizado para encontrar grupos naturales de palabras que aparentemente no tienen relación. 

### Asignación de Dirichlet Latente {#dirichlet}

La Asignación de Dirichlet Latente (LDA) es particularmente popular para encajar un modelo de tópicos[^6]. Toma cada documento como una mezcla de tópicos, y a cada tópico como una mezcla de palabras. Esto permite que una palabra pueda hacer parte de varios tópicos. Matemáticamente, este método estima simultáneamente la mezcla de palabras que están asociadas con cada tópico, al tiempo que identifica la mezcla de tópicos que describen cada documento.

Se convierte la variable a un formato apto para realizar el cálculo del LDA. Se analizarán 2 tópicos potenciales para entender el contenido de palabras en cada uno. 

```
doc <- cast_dtm(data = d, document = depart, term = word, value = V1, weighting = tm::weightTf)
print(doc)
```
<div>
  <a>
    <img src="http://camicabrera.com/img/blog/planesdnp/datos6.png" alt="">
  </a>
</div>

```
ap_lda <- LDA(doc, k = 2, control = list(seed = 1234))
print(ap_lda)
```
<div>
  <a>
    <img src="http://camicabrera.com/img/blog/planesdnp/datos7.png" alt="">
  </a>
</div>

# Probabilidades tópico-palabra

Se comenzará con el análisis por-tópico-por-palabra extrayendo las probabilidades o β (“betas”) del modelo. 

```
ap_topics <- tidy(ap_lda, matrix = "beta")
print(ap_topics)
```

<div>
  <a>
    <img src="http://camicabrera.com/img/blog/planesdnp/datos8.png" alt="">
  </a>
</div>

Este procedimiento convirtió el modelo a un formato de una fila por tópico por palabra. Para cada combinación, el modelo computa la probabilidad que ese término haya sido generado por ese tópico. Por ejemplo, el término "abandon" tiene una probabilidad de 1.71e-04 de haber sido generado del tópico 1 y una del 3.28x-05 de haber sido generado del tópico 2. 

Para cada tópico, se ordenará por mayor probabilidad que la palabra haya sido generada por ese tópico. 

```
ap_top_terms <- ap_topics %>%
  group_by(topic) %>%
  top_n(15, beta) %>%
  ungroup() %>%
  arrange(topic, -beta)

ap_top_terms %>%
  mutate(term = reorder(term, beta)) %>%
  ggplot(aes(term, beta, fill = factor(topic))) +
  geom_col(show.legend = FALSE) +
  facet_wrap(~ topic, scales = "free") +
  coord_flip()
```
<div style="text-align:center;">
  <a>
    <img src="http://camicabrera.com/img/blog/planesdnp/imagen8.png" alt="">
    <img src="http://camicabrera.com/img/blog/planesdnp/imagen9.png" alt="">
  </a>
</div>

Esta gráfica permite entender los dos tópicos que fueron extraidos de los PDTs. Las palabras más comunes en el tópico 1 incluyen "población", "salud", "implementar", "proces", "indic" lo que puede sugerir que están más enfocados a la implementación de proyectos de salud. Aquellas palabras más comunes en el tópico 2 incluyen "social", "econom", "derech", "genero" y "niñ" infiriendo una inclinación hacia estrategias de interacción social (niñez, género, derecho) ajenas a la salud. Aquí se resalta una ventaja del modelaje de tópicos opuesta a los métodos de "conglomerados duros": lo tópicos en lenguaje natural se pueden superponer en términos de palabras. 

Como una alternativa, se podría considerar palabras que tuvieron la mayor diferencia de betas (β) entre tópico 1 y tópico 2. Esta se puede estimar usando el logaritmo del radio entre ambos parámetros. Para limitarlo a un grupo de palabras especialmente relevantes, se filtra por las palabras comunes, como aquellas que tienen una probabilidad (beta) mayor a (1/1000) en al menor un tópico. A continuación las palabras con las mayores diferencias entre los dos tópicos. 

```
beta_spread <- ap_topics %>%
  mutate(topic = paste0("topic", topic)) %>%
  spread(topic, beta) %>%
  filter(topic1 > .001 | topic2 > .001) %>%
  mutate(log_ratio = log2(topic2 / topic1))


  beta_spread %>%
  mutate(term = reorder(term, log_ratio)) %>%
  filter(abs(log_ratio) > 4) %>%
  ggplot(aes(term, log_ratio)) +
  geom_col(show.legend = FALSE) + 
  coord_flip()
```

<div style="text-align:center;">
  <a>
    <img src="http://camicabrera.com/img/blog/planesdnp/imagen10.png" alt="">
  </a>
</div>

Las palabras más comunes en el tópico 2 son: "formacion", "fisic", "conserv", "natural", "direccion", "comunitari", "desplaz", "orden", "potencial", "coordin", "asistent" quizás infiendo soluciones a problemáticas sociales (comunitari, coordin, formacion) y de desplazamiento (fisic, orden, direccion). En el tópico 1 están las palabras "violenci", "viviend", "trabaj", "cultur", "climat", "urban", "saneamient", "tecnolog", "promocion", "interes", "document" posiblemente abordando más temáticas de salud (urban, violenc, climat, saneamient) y convivencia (cultur, trabaj, viviend, tecnolog). 

# Probabilidades documento-tópico {#topico}

Además de estimar cada tópico como una mezcla de palabras, LDA también modela cada documento como una mezcla de tópicos. En el modelo, el estimador de estas probabilidades es llamado "gamma" (γ):

```
ap_documents <- tidy(ap_lda, matrix = "gamma")
ap_documents$gamma <- round(ap_documents$gamma*100, 1)
depart <- dcast.data.table(data = as.data.table(ap_documents), formula = "document ~ topic", value.var = "gamma")
print(depart)
```
<div>
  <a>
    <img src="http://camicabrera.com/img/blog/planesdnp/datos9.png" alt="">
  </a>
</div>

Cada uno de estos valores es una proporción estimada de palabras que vienen de ese documento que son generadas por ese tópico. Por ejemplo, el modelo estima que el 76.9% de las palabras del PDT de Cesar construyen el tópico 2 mientras que el 73.1% del PDT de Casanare conforman el tópico 1.

#### Tópico 1: Proyecto de salud {#salud}

```
head(depart[order(`1`, decreasing = T)], 6)
```

Los PDTs que mejor describen el tópico 1 son Casanare, Vaupés, Huila, Risaralda, Córdoba y Sucre. Resalta la diferencia que existe entre Casanare con un 73.1% respecto a Vaupés (66.6%) y Huila (63.9%). Cuando se retorna a la vista por PDT ponderado por TF-IDF, Casanare resalta de nuevo las palabras "saludsector" y "comunitarioprogram" e infiere una conexión de este tópico con salud. 

#### Tópico 2: Educación y Cultura {#educacion}

```
head(depart[order(`2`, decreasing = T)], 6)
```

Los PDTs que mejor describen el tópico 2 son Cesar, Valle del Cauca, Chocó, Cauca, Amazonas y Magdalena. Al igual que en el tópico 1, hay relativa gran diferencia entre el primero (Cesar con 76.9%) y los demás (Valle del Cauca con 56.9% y Chocó con 56.7%). También se resalta que Valle del Cauca, Chocó y Cauca tengan esa cercanía en este tópico cuando geográficamente son vecinos: es posible que usen los mismos términos en sus documentos. Cesar por su lado resalta las palabras "deeducacionsecretari" y "dedeportesecretari". 

# Apuntes {#apuntes}

La hipótesis resultante del análisis LDA es que el tópico 1 tiene mayor énfasis en proyectos relacionados a la salud y el tópico 2 hacia proyectos sociales de índole educacional y cultural. Aún cuando resaltan palabras pegadas, la frecuencia con la que aparecen y su relevancia latente en comparación con los demás documentos hace que el modelo las tenga en cuenta. Esta hipótesis está sujeta a cambios por la calidad en los datos (palabras unidas). 

Los ganadores del premio del DNP: Antioquia, Caquetá y Nariño tuvieron una distribución equitativa entre ambos tópicos (aprox. 50% en cada tópico) lo que infiere un balance entre los contenidos de salud, educación y cultura de sus documentos y potencialmente podría estar asociado al éxito de sus gobiernos.

# Recomendaciones {#recomendaciones}

1. El hecho que todos los PDT hayan tenido una correlación relativamente alta entre si (superior al 60%), enciende las alarmas de por qué Casanare es el más alejado. Este Departamento rico en petróleo atraviesa [crisis sociales](http://www.colombiainforma.info/la-crisis-que-deja-el-petroleo-en-casanare/) (en especial en salud) y [escándalos de corrupción](https://www.youtube.com/watch?v=UibMVMbO9Jk). Una recomendación sería investigar si este Plan de Desarrollo contiene lineamientos que expliquen los problemas de corrupción o si es necesario rechazar esta hipótesis debido a la calidad de datos disponible.
2. La significativa diferencia de palabras de César respecto a los demás en su tópico destaca la recomendación de investigar a detalle este PDT. 

# Referencias {#referencias}

[^1]: [A Brief Survey of Text Mining: Classification, Clustering and Extraction Techniques](https://arxiv.org/pdf/1707.02919.pdf)
[^2]: [Text Mining with R. TF-IDF](https://www.tidytextmining.com/tfidf.html)
[^3]: [Quantify the similarity of bags of words](https://stats.stackexchange.com/questions/289400/quantify-the-similarity-of-bags-of-words)
[^4]: [Pearson Correlation Coefficient](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)
[^5]: [DNP destacó los planes de desarrollo departamentales](http://www.portafolio.co/economia/gobierno/dnp-destaco-los-planes-de-desarrollo-departamentales-501057) 
[^6]: [Text Mining with R. Topic Modeling](https://www.tidytextmining.com/topicmodeling.html)
