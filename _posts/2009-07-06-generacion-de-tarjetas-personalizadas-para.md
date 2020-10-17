---
layout: post
title: CLI - Generación de tarjeta de acreditación para congresos
date: '2009-07-06T19:44:21+02:00'
tags:
- Utilidades
- BASH
- GNU/Linux
---
En el post anterior se vió como se extraía la información necesaria de documentos .doc. El siguiente script sirve para automatizar la generación de las tarjetas de identificación (acreditaciones) de los participantes del congreso.   
  
 Situacion: Se necesitan imprimir unas 300 tarjetas con el nombre, organización y pais para los participantes de un congreso, acompañantes, y staff... unas 300 acreditaciones aproximadamente.    
  
Este *script* requiere el paquete *imagemagick*. Pide de entrada un archivo .csv (o texto plano) con los siguientes campos separados por comas:PAIS,APELLIDOS,NOMBRE,ORGANIZACION y el modelo de plantilla de acreditación por ejemplo acreditación de "staff", "participante" o "acompañante" etc. 
  
 El resultado es una carpeta llena de ficheros .png de tamaño folio con acreditaciones "cara" y "reverso" idénticas listas para imprimir.   
  {% highlight bash %}
#!/bin/bash
#
#
# Anyone may run this program.  Modification and Distribution of this program are licensed by the author subject to the terms of the Gnu Lesser General Public License.
# You can find the license text here:  http://www.gnu.org/licenses/lgpl.html
# Lee el fichero $INPUT que es un csv separado por comas con
# Pais, Nombre y Apellidos y crea las acreditaciones basadas
# en la plantilla
echo "fichero de lectura?"
read INPUT
echo "tipo de acreditación:"
echo "acomp,staff,part?"
read ACRED
# inicialización de variables.
FONT=../fuentes/VAGROUN.TTF
COLOR1='rgb(49,77,99)'
COLOR2='rgb(253,114,23)'
TARJETAS=$(wc -l < $INPUT)
FOLIOS=$(( $TARJETAS/3 + 1))
#
echo $FOLIOS
# crea carpeta donde van a ir todas las acreditaciones
mkdir folios-$INPUT
sed 's/ ,/,/g' $INPUT | sed 's/, /,/g' | sed "s/'/\\\'/g" | sed 's/,/, /g' > lista
mv lista folios-$INPUT
#
# crea los folios de las acreditaciones, 3 para cada uno.
NFOLIOS=1
while [ $NFOLIOS -le $FOLIOS ]; do
cp $ACRED.png folios-$INPUT/$NFOLIOS.png
let NFOLIOS=$NFOLIOS+1
done
cd folios-$INPUT
#
#
#
LIMIT=$FOLIOS
for ((NFOLIOS=1; NFOLIOS <= LIMIT ; NFOLIOS++))  
do
#nombre y apellidos $3$2
convert $NFOLIOS.png -fill $COLOR1 -font $FONT -pointsize 94 -draw "text 80,847 '$(awk -F, 'NR=='3*$NFOLIOS-2' {print($3 $2)}' lista)'"\
"text 1290,847 '$(awk -F, 'NR=='3*$NFOLIOS-2' {print($3 $2)}' lista)'"\
"text 80,1763 '$(awk -F, 'NR=='3*$NFOLIOS-1' {print($3 $2)}' lista)'"\
"text 1290,1763 '$(awk -F, 'NR=='3*$NFOLIOS-1' {print($3 $2)}' lista)'"\
"text 80,2678 '$(awk -F, 'NR=='3*$NFOLIOS' {print($3 $2)}' lista)'"\
"text 1290,2678 '$(awk -F, 'NR=='3*$NFOLIOS' {print($3 $2)}' lista)'" $NFOLIOS.png
#pais $1
convert $NFOLIOS.png -fill $COLOR1 -font $FONT -pointsize 94 -draw "text 94,1076 '$(awk -F, 'NR=='3*$NFOLIOS-2' {print$1}' lista)'"\
"text 1304,1076 '$(awk -F, 'NR=='3*$NFOLIOS-2' {print$1}' lista)'"\
"text 94,1992 '$(awk -F, 'NR=='3*$NFOLIOS-1' {print$1}' lista)'"\
"text 1304,1992 '$(awk -F, 'NR=='3*$NFOLIOS-1' {print$1}' lista)'"\
"text 94,2908 '$(awk -F, 'NR=='3*$NFOLIOS' {print$1}' lista)'"\
"text 1304,2908 '$(awk -F, 'NR=='3*$NFOLIOS' {print$1}' lista)'" $NFOLIOS.png
#organización $1
convert $NFOLIOS.png -fill $COLOR2 -font $FONT -pointsize 44 -draw "text 90,958 '$(awk -F, 'NR=='3*$NFOLIOS-2' {print$4}' lista)'"\
"text 1300,958 '$(awk -F, 'NR=='3*$NFOLIOS-2' {print$4}' lista)'"\
"text 90,1874 '$(awk -F, 'NR=='3*$NFOLIOS-1' {print$4}' lista)'"\
"text 1300,1874 '$(awk -F, 'NR=='3*$NFOLIOS-1' {print$4}' lista)'"\
"text 90,2790 '$(awk -F, 'NR=='3*$NFOLIOS' {print$4}' lista)'"\
"text 1300,2790 '$(awk -F, 'NR=='3*$NFOLIOS' {print$4}' lista)'" $NFOLIOS.png
echo "numero de folio=$NFOLIOS"
done
#rm -f /home/ejjuarez/DOCUMENTACION-LINUX/MY-macros-scripts/acreditaciones-imeboron/folios/$INPUT
#
#
#
#
  {% endhighlight %}
En la figura de abajo se puede ver el resultado, las dimensiones son aproximadamente 8x10 cm:

| ![](/imgs/8273bd80addfd7eb5e61d3773f4912ce_26935c12_540.jpg)|
|:--:|
|*Ejemplo de acreditación generada para el IMEBORON XIII*|

*Actualización 7 Dec 2017: con modificaciones triviales a este script se puede automatizar la generación de certificados*

| ![](/imgs/certificate-ejjp.jpg)|
|:--:|
|*Ejemplo de certificado generado para el ISEST2018*|


