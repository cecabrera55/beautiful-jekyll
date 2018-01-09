---
  title: "Pronosticando los retornos de una acción"
layout: post
date: '2017-06-15'
published: yes
share-img: /img/posts/stock_images/grafica1.png
tags:
- Español
- Trading
- Stock
- Analytics
- Aprendizaje Automático
- Modelos Estadísticos
- ARIMA
- Pronostico
- Acciones
- Precios
permalink: /blog/stock/
---

En el [archivo de Excel](https://cecabrera.github.io/files/Datos.xlsx) hay información de precios desde el 10 de Abril de 2013 al 26 de Mayo de 2017 de la empresa "XYZ". 
Me dí a la tarea de desarrollar un modelo estadístico que pronosticara si el precio de una acción sube o baja en función de su valor histórico y otras variables. 
Este análisis fue desarrollado en R, Markdown y GitHub y es hecho con propósitos investigativos. El código fuente pueden hallarlo en mi [reposotorio (carpeta)](https://github.com/cecabrera/stock) en GitHub.

#Tabla de ccontenidos
- [Modelo de regresión logística](#regresion)
- [Calculando la habilidad predictiva del modelo](#predictiva)
- [Análisis con modelos ARIMAX](#arimax)
- [Análisis interactivo de 25 modelos](#interactivo)
- [Conclusiones](#conclusiones)
- [Referencias](#referencias)
  
El plan de trabajo de este informe se resume en:
1. análisis de Regresión Logística para encontrar asociaciones entre los indicadores y el precio de la acción;
2. análisis de ARIMAX para modelar el comportamiento de los retornos y calcular la tendencia y magnitud del precio en el corto plazo; 
3. y finalmente una sucesión de códigos en GitHub para evaluar 25 modelos estadísticos en un bucle y determinar cuál tiene la mejor precisión al momento de pronosticar el precio de la acción. 

Comenzamos cargando los paquetes a utilizar y los datos del archivo de Excel:
  ```  
# install.packages(c("caTools", "tseries", "readxl", "data.table", "plotly"))
require(caTools)
require(tseries)
require(readxl)
require(data.table)
require(ggplot2)
require(corrplot)
library(ROCR)

d <- data.table(readxl::read_excel(path = "Datos.xlsx", sheet = 1))

# mostrar las primeras 6 filas de las primeras 7 columnas.
head(d[, .(fecha, open, high, low, close, volume)])
```


