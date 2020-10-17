---
layout: post
title: Utilidades - Conversión de unidades de energía
date: '2009-02-16T10:35:10+02:00'
tags: []

---



Fuente original del código: <a href="http://www.colby.edu/chemistry/PChem/Hartree.html">aquí</a>

<script language="JavaScript">
<!--HIDE FROM OTHER BROWSERS
//DEFINE METHODS
function constants(conv) {
var numE = 7;
conv[0] = 1.000 ;
// eV
conv[1] = 2.7211399E+01 ;
// kJ/mol
conv[2] = 2.6255002E+03 ;
// kcal/mol
conv[3] = conv[2]/4.184 ;
// cm-1
conv[4] = 2.1947463E+05 ;
// V
conv[5] = 2.6255002E+06/96484.6 ;
// K
conv[6] =3.1577709E+05 ;
// Boltzman
conv[7] = -conv[6] ;
return numE;
}
function displayInfo(form,field) {
// find field index
for (var i=0; i<=nfields; i++) {
if ( form.elements[i].name == field ) {
idx = i ;
break;
}
}
// find number of characters in input string for significant figure functions
nchars = form.elements[idx].value.length +1 ;
// calculate the base energy in Hartrees
if ( idx != 7) {
energy = form.elements[idx].value/conv[idx];
} else {
energy = Math.log(form.elements[idx].value)*298.15/conv[idx];
}
// convert to other units
for (var i=0; i<=nfields; i++) {
if ( i != idx ) {
if ( i != 7) {
form.elements[i].value = trunc(energy*conv[i],nchars) ;
} else {
form.elements[i].value = trunc(Math.exp(energy*conv[i]/298.15),4) ;
}
}
}
boltzman()
}
function boltzman() {
// calculate boltzman fractions and voltage for general conditions
var T = document.Boltzman.T.value ;
var gj = document.Boltzman.gj.value ;
var gi = document.Boltzman.gi.value ;
var z= document.Boltzman.z.value ; var r = Math.exp(energy*conv[7]/T)*gj/gi ;
document.Boltzman.flow.value = trunc(1/(r+1)*100.0,3) ;
var fup = trunc(r/(r+1)*100.0,3) ;
if ( fup > 1e-20 ) {
document.Boltzman.fup.value = fup } else {
document.Boltzman.fup.value = 0 }
// Put in diagram poulations
var all = "-oooooooooo" ;
var molecules = Math.floor(r/(r+1)*10.0+0.5) ;
document.Boltzman.up.value = all.substring(0,molecules+1) + "-" ;
document.Boltzman.low.value = all.substring(0,11-molecules) + "-" ;
// Voltage for z != 1
document.Boltzman.V.value = trunc(energy*conv[5]/z,nchars) ;
}
// Significant figure functions
function ord(x) {
return Math.floor(Math.log(Math.abs(x+1e-35))/2.303)
}
// Truncate to n sign. figures
function trunc(x,n) {
return Math.floor(x*Math.pow(10,-ord(x)+n-1)+.5)/Math.pow(10,-ord(x)+n-1)
}
// MAIN variable declarations
var energy = 0.000;
var nchars = 0;
var conv = new Array();
var nfields = constants(conv);
// STOP HIDING FROM OTHER BROWSERS -->
</script>


### Energy Units Converter
Enter your energy value in the box with the appropriate units, then
press "tab"
or click outside of the input box.
<p></p>
<form name="Hartree" method="post"><input name="H" value="0" onchange="displayInfo(this.form,this.name);" type="text">Hartrees<br>
<input name="eV" value="0" onchange="displayInfo(this.form,this.name);" type="text">eV<br>
<input name="kJ/mol" value="0" onchange="displayInfo(this.form,this.name);" type="text">kJ/mol<br>
<input name="kcal/mol" value="0" onchange="displayInfo(this.form,this.name);" type="text">kcal/mol<br>
<input name="cm-1" value="0" onchange="displayInfo(this.form,this.name);" type="text">cm<sup>-1</sup><br>
<input name="V" value="0" onchange="displayInfo(this.form,this.name);" type="text">V
for 1e<sup>-</sup> transfer<br>
<input name="K" value="0" onchange="displayInfo(this.form,this.name);" type="text">K
(equivalent temperature)<br>
<input name="B" value="1" onchange="displayInfo(this.form,this.name);" type="text">Boltzman
population ratio at 298.15K
</form>



