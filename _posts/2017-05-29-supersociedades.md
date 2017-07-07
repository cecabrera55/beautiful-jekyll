---
layout: post
title: Oportunidad de inversión en I+D+i para las empresas colombianas
tags:
  - Español
  - Beneficios tributarios
  - I+D+i
  - Colombia
  - Productividad
  - Ahorro tributario
published: false
---

El entorno empresarial colombiano padece una enfermedad económica crónica: la brecha de la productividad. La desarticulación de actores y la falta de talento humano, entre otros, limitan nuestra capacidad de competir en un entorno global. Este blog resume la situación actual de Colombia en torno a la productividad y muestra una propuesta metodológica para solucionar el problema. Está sustentado a su vez en la tesis de maestría de Camilo Cabrera en 2015 disponible en [español](files/Tesis ESP.pdf) y en [inglés](files/Tesis ENG.pdf). 

## La brecha de la productividad

Un trabajador en Colombia produce 4 veces menos dinero que un trabajador en Estados Unidos y esta tendencia se ha mantenido en los años. 

![Productividad Laboral total relativa de Colombia frente a Estados Unidos (EEUU=100%)](img/posts/HistoricoProductividad.png)

Si medimos la productividad laboral en Estados Unidos en 100% (medidos en USD producidos por hora trabajada, por ejemplo) y comparamos la misma cifra en Colombia, estamos en promedio al 25%. La [situación en América Latina](http://www.larepublica.co/el-mercado-local-tiene-el-peor-registro-de-productividad-laboral_159976) es relativamente similar.

![Productividad laboral en la región latinoamericana](img/post/productividad/ProductividadLatam.png)

Ese 75% de diferencia es lo que denominamos la brecha de la productividad. Al ser relativa a un país altamente productivo como Estados Unidos, no tiene el sesgo subjetivo de ser medido por aumentos porcentuales de 1 o 2 puntos sino por un referente en el mercado sólido.

## ¿para qué cerrar la brecha? 

Esta situación impacta directamente la calidad de vida de los ciudadanos. El promedio de horas trabajadas anualmente en un país como Colombia es de 2.770 horas mientras que en Estados Unidos es de 1.788. Las economías desarrolladas tienen índices de pocas horas laborales, pues van de la mano la calidad de vida de los trabajadores y su productividad.

En Noruega se trabajan 27 horas semanales, en Francia 35 y Holanda 36. En Latinoamérica, Chile trabaja en promedio 38 horas semanales, Ecuador 40 horas y Colombia aproximadamente 53 horas. “La tendencia mundial sobre las horas que debe laborar una persona al año demarca que cada vez se reducen las jornadas, pues está demostrado que a mayor número de horas, menor es la productividad”.  Referencia [aquí](http://www.larepublica.co/node/180481).

![Productividad laboral relativa de los sectores económicos en Colombia (frente a Estados Unidos)](img/post/productividad/McKinsey.png)

Las empresas informales son las menos productivas. Esto tiene implicaciones en la calidad de vida de los colombianos y en las finanzas del Estado, por su impacto vía impuestos perdidos y por los sobrecostos de la informalidad laboral, al no recaudarse una buena parte de los aportes a salud y pensiones previstos.

Finalmente, esta brecha de productividad es el principal obstáculo para aprovechar plenamente las oportunidades que ofrece la apertura comercial y el libre comercio y por ello debe ser el foco estratégico de las políticas encaminadas al desarrollo productivo. Como solución a esta problemática, el Banco Interamericano de Desarrollo propone que la solución a la brecha de la productividad está en la inversión en I+D+i enfocada a una Transformación Productiva (Crespi, Fernández-Arias, & Stein, 2014).

## Articulando los actores

El problema de la brecha de la productividad se resuelve a través de la inversión en I+D+i. Las empresas están buscando nuevas formas de innovar en sus procesos, el Estado a través de incentivos tributarios promueve la inversión en I+D+i y las universidades son foco de conocimiento científico para aportar al sector real. Entonces, ¿por qué estamos tan quedados?

Existes varios motivos. Uno importante es la ausencia de capital humano capacitado en la estructuración de proyectos de inversión en I+D+i. Colciencia, a través del CNBT (Consejo Nacional de Beneficios Tributarios) certifica proyectos catalogados como I+D+i y, según el artículo 158-1 del [estatuto tributario](http://estatuto.co), las personas que inviertan en dichos proyectos tienen derecho a descontar el 175% de la inversión sobre el impuesto de renta siempre y cuando el valor no exceda el 40% de la renta líquida gravable. 

### Veamos un ejemplo
__hay que organizar mucho mejor este párrafo pues es vital__
Una empresa planea vender 100 millones en el 2017. Asumiendo una tasa tributaria efectiva del 33% tendria que pagar 33 millones a la Dian al final el próximo año. Si este año decido invertir 10 millones en un proyecto de I+D+i en mi empresa, entonces puedo deducir 17.5 millones de los 33 que debería pagar. Es decir, al final del ejercicio tendría que pagar solo `33 - 17.5 = 15.5 millones` y estaría al tiempo invirtiendo en un proyecto para mi compañía. Supongamos que mi renta líquida gravable (equivalente al EBITDA) este año va a ser de 40 millones. Como la deducción (17.5) no excedió el 40% de mi renta líquida gravable (40 millones) puedo deducir 

La Superintendencia de Sociedades de Colombia conglomera la información tributaria y financiera de la mayoría de empresa formales en Colombia. Su base de datos tiene registros desde 2004 hasta la fecha. En el 2013, un amigo y yo tomamos el reto de programar un bot que recibiera como input la 

Este proyecto salió de mi tesis de maestría: cómo cerrar la brecha de la productividad que existe en la industria colombiana respecto a sus competidores internacionales. La metodología se reduce en utilizar el incentivo de beneficios tributarios por inversión en I+D+i (investigación científica, desarrollo tecnológico e innovación) para que las empresas estructuraran este tipo de proyectos _in-house_. El objetivo fue encontrar la información financiera, revisor

La data de sus sistemas tiene información de estados financieros, balances generales y estados de pérdidas y ganancias de más de 26 000 empresas, además de la información de representantes legales, revisores fiscales y contadores (quienes son los que reportan estos datos a la supersociedades). 

En alianza con un amigo de la universidad, también curioso por los datos, desarrollamos una App web con el ánimo de hacer toda esta información, además de disponible para los ciudadanos, digerible para aquellos que quieran indagar a detalle sobre la data disponible. 

Se utiliza una técnica llamada_webscrapping_ para "gatear" (_to crawl_ en inglés) en las páginas web con el ánimo de automatizar la descarga de contenidos. En esencia, es crear un bot para que esculque adentro del código HTML de la página y, en algunos casos,  simule el comportamiento humano (click, scroll, interacciones) a través de Javascript para descargar información iterativamente.  

El código fuente fue desarrollado en aproximadamente 8 horas de trabajo de Andrés Torres y el Dashboard que están por presenciar tardó aproximadamente 16 horas en salir al aire. 

Hacer análisis de cluster a las empresas usando las variables de ventas, número de empleados, años disponibles en la supersociedades, número de activos, deuda, patrimonio 

Hemos llegado a una era en la que tener la información no es suficiente para aportar valor a las personas. Hace falta un capa adicional de analítica para encontrar patrones ocultos en la data y visualizar métricas relevantes para la toma de decisiones. 

Apuntes finales 

