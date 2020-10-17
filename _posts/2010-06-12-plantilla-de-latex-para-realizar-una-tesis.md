---
layout: post
title: LATEX - plantilla para tesis doctoral por compendio de artículos
date: '2010-06-12T19:54:45+02:00'
tags:
- Utilidades
- LATEX
---
Esta plantilla está basada en la plantilla  itsas\_thesis\_template.es.r23.tar.gz con modificaciones simples para adaptarla a los requerimientos del departamento de Química de la UAB (2009).

#### Instalando L<sup>A</sup>T<sub>E</sub>X.

Esta primera parte está dedicada a la instalación de L<sup>A</sup>T<sub>E</sub>X. Si ya lo tienes instalado puedes ir al siguiente apartado donde se explica como usar la plantilla.

Mi ordenador tiene una Debian 5 (Lenny) aunque los pasos que se exponen a continuación deben ser válidos para toda distro moderna.

Para crear código L<sup>A</sup>T<sub>E</sub>X que luego compilaremos realmente sólo se necesita un editor de textos tipo "bloc de notas", sin embargo existen entornos como "kile" que facilitan el trabajo de edición de estos ficheros .tex

{% highlight bash %}
  $ sudo aptitude install kile
{% endhighlight %}



  

Tras instalar kile procedemos con la instalación de L<sup>A</sup>T<sub>E</sub>X. Es recomendable instalar estos paquetes y todos los que sugiera *aptitude*,
{% highlight bash %}
    $sudo aptitude install texlive texlive-LATEX-extra texlive-science texlive-lang-spanish

{% endhighlight %}
Además de todo lo instalado anteriormente se necesitan dos paquetes de L<sup>A</sup>T<sub>E</sub>X que no he podido localizar en paquetes de debian. Sin embargo es sencillo conseguirlos en CTAN. Los paquetes en cuestión son mciteplus y notes2bib:
{% highlight bash %}
    $wget [http://www.ctan.org/get/macros/LATEX/contrib/mciteplus.zip](http://www.ctan.org/get/macros/LATEX/contrib/mciteplus.zip)
    $wget [http://www.ctan.org/get/macros/LATEX/contrib/notes2bib.zip](http://www.ctan.org/get/macros/LATEX/contrib/notes2bib.zip)
{% endhighlight %}
descomprimimos los .zip

y dentro de la carpeta notes2bib debemos generar el fichero .sty de la siguiente manera:
{% highlight bash %}
    $latex notes2bib.ins
{% endhighlight %}
tras esto llevamos a /usr/share/texmf/tex/latex/ las dos carpetas para tenerlos disponibles para todo el sistema,
{% highlight bash %}
    $sudo cp -r mciteplus /usr/share/texmf/tex/latex/
    $sudo cp -r notes2bib /usr/share/texmf/tex/latex/
{% endhighlight %}
finalmente hacemos el *update* de los paquetes de L<sup>A</sup>T<sub>E</sub>X
{% highlight bash %}
    $sudo texhash
{% endhighlight %}
A continuación abrimos kile y ya estamos listos para utilizarlo. Pero antes, para poder usar correctamente la plantilla de la tesis tenemos que configurar kile para que use de codificación iso-8859-15 en lugar de la utf8 que usa por defecto. Todos los ficheros .tex de esta plantilla están en la primera codificación y se usa \usepackage[latin1]{inputenc} para poner acentos y caracteres especiales del castellano como las eñes. Si abrimos un fichero .tex codificado como iso-8859-15 en un kile con utf8 veremos que tildes y eñes han sido sustituidas por símbolos de interrogación. En Settings - configure kile... - editor – open/save y en file format y en encoding seleccionamos iso-8859-15 y guardamos. Ya está todo preparado.

#### Como usar esta plantilla de L<sup>A</sup>T<sub>E</sub>X.

Descargamos el fichero [TESIS-plantilla.tar.bz2](https://docs.google.com/open?id=0BwtC68TRVCNDaTZmZi15UFhIZGM) y lo descomprimimos. La carpeta contiene 18 ficheros tipo .tex, 1 fichero .bib (la bibliografía), una carpeta llamada Config (con ficheros de configuración) y una carpeta donde se guardan las figuras que se vayan a utilizar para la elaboración del documento.

El documento maestro de esta plantilla se llama PRINCIPAL.tex. Podemos abrir este fichero en kile e ir leyendo para ver como está estructurado. Este fichero PRINCIPAL.tex es el que compilaremos para generar el pdf. Mediante órdenes *include* se incorporan a PRINCIPAL.tex el resto de secciones y capítulos (otros ficheros .tex) que formarían la tesis.

La normativa obliga a que este documento tenga algunas partes de obligada inclusión. Básicamente la tesis la conforman los siguientes apartados:

- 1portada.tex ( portada con logos en ciertas posiciones, título, autor, director…)
- 2certificado-extra.tex (certificado que exige el departamento)
- 2certificado.tex (certificado que ponemos en el CSIC, con membrete)
- 3financiacion.tex (hoja donde se indican las becas…etc)
- 4agradecimientos.tex (las hojas de agradecimientos)
- 5prefacio.tex (donde se explica que la tesis es por artículos y se relacionan los que pasaron la comisión)
- 6figurascompuestos.tex (hojas con los esquemas de los compuestos)
- 7abreviaturas.tex (hoja de abreviaturas)
- 8resumen.tex (un resumen de la tesis)
- 9toc.tex (el indice general, este fichero no hay que rellenarlo con nuestros datos simplemente es un fichero de configuración para generar automáticamente el índice. Con un *include* a este fichero en PRINCIPAL.tex indicamos la posición del índice en la tesis)
- x1-Introduccion.tex (capítulo con la introducción)
- x2-Objetivos.tex (capítulo con los objetivos)
- x3-RYD.tex ( primera hoja indicando el comienzo de Resultados y Discusion)
- x4cap.tex (primer capítulo de RyD, crear cuantos sean necesarios y hacer un *include* en PRINCIPAL.tex donde corresponda)
- x7-Conclusion.tex (hojas con las conclusiones
- x8-Publicaciones-A.tex (las publicaciones que pasen la comisión son un capítulo de la tesis, las otras publicaciones que queramos incluir deben ir en un anexo, ver x9-Publicaciones-B.tex)
- x9-Publicaciones-B.tex (anexo con otras publicaciones)
- tesis.bib (este fichero contiene las referencias en formato bibtex, recomiendo JabRef como base de datos para referencias en bibtex)

Una vez seleccionado PRINCIPAL.tex como documento maestro en kile, generamos el pdf. Si todo fue bien instalado se debería de producir un pdf de unas 60 páginas donde he dejado algunas figuras, una tabla y citas en el texto a modo de ejemplo. Ya sólo queda ir rellenando cada fichero .tex con tu material y una vez compilado obtener un .pdf con la tesis.

La inclusión de los artículos en la tesis merece un inciso especial. Lo más fácil sería una vez obtenido el pdf de la tesis ir anexando en ese mismo pdf los ficheros pdf que corresponden a los artículos, pero esto rompería la paginación, cabeceras, pies de página etc. Un método más apropiado para incluirlos en el documento fue deshacer cada pdf con un artículo en sus hojas por separado también en formato .pdf. (recomiendo el paquete pdftk para hacer esto). De esta manera, incluiremos cada hoja de los artículos una a una como si fueran figuras con la orden correspondiente (ver x9-Publicaciones-B.tex por ejemplo) He dejado la primero página de alguno de los artículos para que se entienda el proceso. Cada hoja de cada artículo debe ser guardada en el lugar correspondiente dentro de la carpeta *figuras*.

