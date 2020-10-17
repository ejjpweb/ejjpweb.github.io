---
layout: post
title: SOFT - G3data
date: '2012-03-29T13:59:00+02:00'
tags:
- Software
---
 

### Utilidades: [G3data](https://packages.debian.org/search?keywords=g3data)


G3data[^1] se utiliza para extraer los datos de los gráficos en figuras. En las publicaciones se suelen incluir gráficos, pero faltan los datos reales. Con G3data, el proceso de extracción de estos datos es sencillo y el programa puede leer muchos formatos de imagen diferentes.


A continuación se muestra un uso práctico del programa.


Durante la pasada huelga del 29 de marzo (2012), muchos medios y páginas webs comenzaron a seguir los datos de [REE](https://demanda.ree.es/demanda.html) sobre demanda de energía electrica para medir el seguimiento que estaba teniendo dicha huelga. Por ejemplo: [éste](http://www.economistasfrentealacrisis.com/), [éste](http://politikon.es/2012/03/29/estimando-el-seguimiento-de-la-huelga-en-tiempo-real/) y [éste](http://www.joserodriguez.info/bloc/?p=5402).  
  
Los resultados que ofrecen son bastante dispares (e incluso contradictorios entre sí, por ejemplo entre los dos primeros).  Aunque en general la metodología es buena, creo que en ningún momento se está integrando áreas bajo la curva de demanda. Entonces, he decidido hacer mi propia cuantificación usando G3data y comparar demandas durante esta huelga y la del 29/9/2010.

Se podría considerar que existe un 100 % de seguimiento de la huelga si la demanda presenta una curva similar a la de un domingo (por ejemplo el domingo anterior) y un seguimiento del 0% si la curva integrada es similar a la del dia anterior a la huelga. 
  
Los gráficos que proporciona [REE](https://demanda.ree.es/demanda.html) son muy atractivos visualmente y bastante interactivos. 



|![](/imgs/m1nan1Iwyt1rsb0g7o1_r1_1281.png)   |
|:--:|
|*Demanda en MW. Fuente: REE.*|



Pero a la hora de intentar extraer los datos en crudo resulta tedioso hacerlo directamente del gráfico. Desconozco si hay otra forma de hacerlo que resulte más sencillo pero con  [G3data](https://packages.debian.org/search?keywords=g3data) el proceso de extraer datos de los gráficos es trivial.

Su uso es bastante sencillo, primero imprimimos la pantalla a un fichero en formato imagen (png, tiff). Segundo, se indican donde están situados los ejes *x* e *y* del gráfico y se define su escala. Finalmente, con la herramienta *zoom* vamos marcando con *clics* de ratón la curva con la precisión que se desee, por ejemplo con unos 100 puntos por línea.   
  
En la siguiente figura se muestran las gráficas extraidas usando G3data, el día previo a la huelga, el domingo previo, el día de la huelga actual y el dia del huelga del 29/09/2010. 


|![](/imgs/m1nan1Iwyt1rsb0g7o1_r1_1280.png)   |
|:--:|
|*Demanda en MW. Fuente: REE.*|

Con los datos extraidos integramos las áreas bajo la curva obteniendo energía consumida entre las 0-24 horas en MWh como mostrado en la siguiente tabla:

|---
| Día | Consumo (MWh)  
|:-:|:-:|
|  Domingo previo 25| 581.2   
|Miércoles 28 (día previo)| 689.3   
|Jueves 29 (huelga)| 592.4   
|anterior huelga (29/9/2010)|  604.6 
|---


Con estos datos obtenemos una estimación del seguimiento de la huelga del 29/3/2012 y la 29/09/2010 de aproximadamente 89 y 78 %, respectivamente.


---
[^1]: The original G3data was fetched from [http://www.frantz.fi/software/g3data.php](http://www.frantz.fi/software/g3data.php). Now G3data is available as a [Debian package](https://packages.debian.org/search?keywords=g3data).




