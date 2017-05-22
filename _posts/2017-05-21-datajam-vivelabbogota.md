---
layout: post
title: "DataJam en Bogotá"
tags:
  - Español
  - Datos abiertos
  - Github
  - HTML5
  - ViveLabBogotá
published: true
bigimg: "/img/posts/semaforos.png"
---
[DataJam: Datos DC Suba](http://datosdc.vivelabbogota.com/) fue un evento realizado el 20 y 21 de mayo en el Punto Vive Digital de Suba. Organizados por la Alta Consejería Distrital de TIC en alianza con ViveLabBogotá y la Universidad Nacional de Colombia, esta serie de eventos buscan que el asistente aprenda, proponga y prototipe ideas para la resolución de problemas reales mediante el uso de [datos abiertos](http://www.datos.gov.co) y el desarrollo de mashup apps. 

## La agenda
La agenda del evento se dividió en dos secciones principales; y mucha comida (guiño para el staff). La primera parte tardó medio día e incluyó una actividad de integración llamada ["la fábrica de pelotas"](https://www.youtube.com/watch?v=-tMcQBfJPOo) liderada por Danny Sierra, gestor del proyecto. Vimos talleres de HTML, CSS, Github, Javascript, Jquery, D3.js y Google Maps para trabajar con las [fuentes de datos](http://datos.gov.co) (datasets) abiertos del Gobierno Nacional. Dada la limitante de tiempo, cada taller tuvo una mirada holística a su lenguaje con el propósito de profundizar mediante la práctica en la segunda parte del fin de semana.

Durante el siguiente día y medio, las actividades se enfocaron en armar un equipo, seleccionar los datos, construir una solución a un problema, y desarrollar un prototipo con la información disponible. El equipo lo constituimos [Luis Alejandro Mahecha](https://www.linkedin.com/in/lamahechag/), [Sebastián Malagón](https://www.linkedin.com/in/sebasti%C3%A1n-malag%C3%B3n-p%C3%A9rez-17768b125/), [Alexander Vargas](https://www.linkedin.com/in/alexander-vargas-7b428111b/) y [yo](http://cecabrera.github.io).

Los retos que debíamos escoger en este DataJam fueron:
1. ¿Cómo reducir los tiempos de desplazamiento de los habitantes de la ciudad?
2. ¿Cómo mejorar la convivencia, aportar a la reconciliación y fomentar la paz entre los habitantes de Bogotá?

Todos en el equipo concordamos en atacar el primer reto: ayudar a los ciudadanos de Bogotá y/o a las entidades prestadoras de servicios de transporte público a reducir los tiempos de desplazamiento.

## El proyecto

El proceso fue divertido. Utilizamos una combinación de códigos en R y Python para lectura, procesamiento y limpieza de datos; y JavaScript, CSS y HTML para visualización y estética de la solución. Ganamos el Curso de Introducción de Tableau Desktop Professional y la Licencia de prueba Tableau Desktop Professional durante 40 días para cada miembro del grupo.

Antes de mostrales nuestro 'mínimo producto viable', debo advertirles que este es el resultado de __relativamente pocas horas de trabajo de personas que se acababan de conocer__. Lo publicamos en dos servidores:

1. En [ViveLabBogotá](http://www.vivelabbogota.com/datosdcsuba02/) para exponerlo ante los jurados.
2. En [GitHub](https://cecabrera.github.io/datosbogota/) para dejarlo a disposición de la comunidad y hacer futuras modificaciones; aprovechando las [bondades](https://cecabrera.github.io/2017-05-14-github-red-social-meritocracia/) de publicar proyectos en GitHub.

Divimos el proyecto en 3 métricas relevantes:
1. [Mapa de calor](https://cecabrera.github.io/datosbogota/semaforos.png). Identifica los puntos donde más se aglutinan los semáforos en Bogotá. En este caso era interesante ver cómo Chapinero, a través de la Caracas, era la zona más densa de semáforos. Desarrollado en R y Javascript.
2. [Mapa de puntos](https://cecabrera.github.io/datosbogota/mapa2.html). Muestra los destinos hacia donde llegan los bogotanos en cuyo color cambia en función de la velocidad promedio recorrida. De nuevo, Chapinero demuestra ser una zona de gran afluencia social en Bogotá. Lenguajes: Python y JavaScript.
3. [Análisis de distancias](https://cecabrera.github.io/datosbogota/Mejorar%20tiempo%20de%20desplazamiento.html). A través de un Notebook de Python (Jupyter), desarrollamos el código para filtrar, analizar y visualizar las métricas claves en el comportamiento de los datos. Es aquí donde encontramos inconsistencias en la recolección de datos, una gran cantidad de personas viajando menos de 15 km por trayecto y una oportunidad para mitigar el tiempo de viaje de los ciudadanos a través de la inversión en ciclorutas y cicloparqueaderos; especialmente en Chapinero. Lenguaje: Python.

La aplicación final fue desarrollada combinando HTML con la plantilla Bootstrap de CSS y el paquete JQuery de JavaScript. El código fuente y detalles técnicos pueden encontrarlos en nuestro [repositorio público](https://github.com/cecabrera/datosbogota) en GitHub.

## Aprendizajes
* En proyectos de ciencia de datos, la limpieza de los mismos consume el 80% del tiempo. Organizar los tipos de caracteres, las coordenadas geográficas, la exportación a formato amigable para Javascript y los filtros a los datos tomaron la mayor parte del fin de semana.
* Integrar front-end y back-end en pro de una solución práctica basada en datos. Nuestra prioridad en esta primera iteración fue entregar un mínimo producto viable, no un producto para la NASA. Fue el mejor aprendizaje de implementar una metodología de [desarrollo ágil de software](https://en.wikipedia.org/wiki/Scrum_(software_development)).
* Git es fundamental para el desarrollo de proyectos digitales. A pesar que todos éramos nuevos en esta tecnología, dimos el voto de confianza y alcanzamos resultados coordinados; sin conflicto de versiones. 

## Apuntes finales
En conclusión, el enfoque ágil de la actividad, la capacidad logística del equipo de ViveLabBogotá y la expertiz técnica de los profesionales de la Universidad Nacional hizo del DataJam un evento aplicado a ciencia de datos. Habilidades como el desarrollo de aplicaciones, el trabajo en equipo y sobre todo la entrega de resultados bajo presión fueron la batuta que orquestó nuestro fin de semana. Tomen esto como una invitación para que aquellos entusiastas de datos se [inscriban](http://datosdc.vivelabbogota.com) a los próximos DataJam's y aprovechen esta oportunidad. Es gratis.