---
layout: post
title: CLI - Extracción de datos de ficheros formulario word
date: '2009-07-06T19:44:21+02:00'
tags:
- Utilidades
- BASH
- GNU/Linux
---
Este script se utilizó en un caso real para automatizar la extracción de información de archivos .doc usados a modo de formulario. 
  
El caso real son unos 200 ficheros .doc tipo abstracts para un congreso. Cada uno de ellos consistía de una sola hoja con Título del poster/conferencia, Autores, Fotografía del autor principal, resumen y referencias. Con los datos extraidos se crearon hojas de cálculo o CSVs para facilitar la creación de listados por paises, direcciones de correo, instituciones, etc. 

El formato .doc no es amigo de la línea de comandos y previo a todo procesado, los ficheros .doc se convirtieron a texto plano .txt usando la macro de Danny Brewer para convertir en *batch* documentos de un formato a otro. 

*Actualización 7 Oct 2018: posiblemente el paquete pandoc sea una buena alternativa a esta macro*


Si el formulario se rellenó siguiendo las instrucciones, el archivo .txt contendría unas determinadas líneas con la información bien localizada. El siguiente paso es aplicarle el *script* detallado más abajo para extraer los datos que necesitamos de esos 200 ficheros de tipo txt. Mucha gente tiene problemas para seguir unas instrucciones paso a paso, quizás porque dichas instrucciones son ambiguas o quizás gusta de romper las normas cuando esto no tiene realmente consecuencias sin descartar que simplemente dicha persona sea un poco faltusca. Aproximadamente el 25 % de los formularios recibidos necesitaron de alguna correción manual.   


 El script usa *sed*, *awk*, *head*, *tail*,... comandos estándar en distribuciones GNU-linux, y está dividido en cuatro partes:

1. La primera parte crea el archivo TITLES.CSV que da correspondencia entre títulos y un numero identificativo que queda grabado en el fichero *lista*.
2. La segunda parte crea AUTHORS.CSV y en este caso da correspondencia entre autores y el número de abstract de *lista*.
3. La tercera parte es la más difícil. Extrae uno a uno todos los autores y le da correspondencia con el abstract que presentan. La dificultad reside en el número de autores variable para cada abstract y su composición que puede ser Nombre + 2 apellidos o +1 apellido, 2 nombres un apellido, 2 nombres + 2 apellidos etc. El script 3 recoge casi toda la casuística pero contra los John Smith III Jr., el nexo "der" o los apellidos de alto abolengo unidos por "de", el script fallará miserablemente. Se recomienda no enmendar este error manualmente o incluso empeorarlo, si es posible, con algún juego de palabras. La función de este tercer script es generar el fichero INDEX.CSV con filas de Apellido, Nombre de tal forma que  contiene todos los autores y todos los abstracts correspondientes en los que Apellido, Nombre aparece como autor.
4. La cuarta parte del script relaciona autores y títulos.
{% highlight bash %}
#!/bin/bash
# Script para el ImeboronXIII que extrae la informacion de los "Abstracts" en
# formato .doc y crea listados .csv's mucho más útiles ;)
# Este script come archivos de texto plano, por ejemplo tipo .txt. Para convertir en batch los .doc en 
# ficheros .txt se puede utilizar la macro de
# Danny Brewer(2003) modified by Dan Horwood, 05/2006, to support new OpenDocument files..  All rights reserved.
# 
# Anyone may run this program.  Modification and Distribution of this program are licensed by the author subject to the terms of the Gnu Lesser General Public License.
# You can find the license text here:  http://www.gnu.org/licenses/lgpl.html
#
#
# Renombra todos los archivos .txt quitando espacios.
rename 's/ //g' *txt
#
#...............script1.........................
#
#
#se crea el archivo "lista" y se obliga a introducir el número de archivos que hay.
ARCHIVOS=`ls *txt -1 > lista`
nl lista
echo "numero de archivos?"
read NUMARCHIVOS
#
# rutina para la creacion de correspondencia entre numero de poster/presentacion..etc y su título
NUM=1
rm -f TITLES.csv
while [ $NUM -le $NUMARCHIVOS ]; do
ARCHIVOTXT=`head -$NUM lista | tail -1`
TITLE=`head -1 $ARCHIVOTXT | tail -1` 
echo "P"$NUM"% "$TITLE >> TITLES.csv
let NUM=$NUM+1
done
#
#...............script2.........................
#
#
# rutina para la creacion de correspondencia entre numero de poster/presentacion..etc y sus autores.
NUM=1
rm -f AUTHORS.csv
rm -f O-names.csv
#
while [ $NUM -le $NUMARCHIVOS ]; do
ARCHIVOTXT=`head -$NUM lista | tail -1` 
NAMES=`head -3 $ARCHIVOTXT | tail -1` 
#
echo "P"$NUM"@ "$NAMES >> O-names.csv
let NUM=$NUM+1
done
#
# Quitamos con sed el típico 'and' del último y penúltimo autor. Cambiamos comas por %. Añadimos % al final de linea
# y quitamos puntos.
#
sed 's/ and /% /g' O-names.csv > 1-names.csv
sed 's/, /% /g' 1-names.csv > 2-names.csv
sed 's/$/%/g' 2-names.csv > 3-names.csv
sed 's/\.//g' 3-names.csv | sed 's/,//g' | sed 's/ % /% /g' > 4-names.csv
#Falta añadir un sed que ponga puntos en las abreviaturas de los segundos nombres.
rm O-names.csv
rm 1-names.csv
rm 2-names.csv
rm 3-names.csv
mv 4-names.csv AUTHORS.csv
#
#
#...............script3.........................
#
#
# 
# el awk: la joya.
rm -f INDEX.csv
awk '{ for (i = NF; i > 2; i--) 
if ($i ~ /%/ && $(i-2) !~ /%/ && $(i-2) !~ /@/) print($1" "$i", "$(i-2)" "$(i-1))}
{ for (i = NF; i > 2; i--) 
if ($i ~ /%/ && $(i-2) !~ /%/ && $(i-2) ~ /@/) print($1" "$i", "$(i-1))}
{ for (i = NF; i > 2; i--)
if ($i ~ /%/ && $(i-2) ~ /%/) print($1" "$i", "$(i-1)) }' AUTHORS.csv | sed 's/@/,/g' | sed 's/%//g' > INDEX.csv
#
#
#...............script4.........................
#
#
# 
# union de Titles y Authors para dar un csv sin número
paste --delimiters="%" TITLES.csv AUTHORS.csv | sed 's/@/%/g' > TA.csv


  
  {% endhighlight %}

