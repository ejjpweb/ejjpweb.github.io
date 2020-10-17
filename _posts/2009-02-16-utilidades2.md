---
layout: post
title: Utilidades - Calculadora de pesos moleculares
date: '2009-02-16T10:35:10+02:00'
tags: []

---


Fuente original del código java de esta calculadora: <a href="http://www.humboldt1.com/%7Emedusa/page/molecalc/">aquí</a>

Pesos atómicos usados: Pure Appl. Chem., 78, 2051-2066 (2006) 

ejemplo: Cu(CH2H3O2)2H2O 


<script language="javascript">
// Masses come from http://www.chem.qmul.ac.uk/iupac/AtWt/
// "based on the 2005 table at Pure Appl. Chem., 78, 2051-2066 (2006) // with 2007 changes to the values for lutetium, molybdenum, nickel, ytterbium and zinc"
// Feel free to keep me updated with new values!
atom=new Array();
atom["H"]= 1.00794;
atom["He"]= 4.002602;
atom["Li"]= 6.941;
atom["Be"]= 9.012182;
atom["B"]= 10.811;
atom["C"]= 12.0107;
atom["N"]= 14.0067;
atom["O"]= 15.9994;
atom["F"]= 18.9984032;
atom["Ne"]= 20.1797;
atom["Na"]= 22.98976928;
atom["Mg"]= 24.305;
atom["Al"]= 26.9815386;
atom["Si"]= 28.0855;
atom["P"]= 30.973762;
atom["S"]= 32.065;
atom["Cl"]= 35.453;
atom["Ar"]= 39.948;
atom["K"]= 39.0983;
atom["Ca"]= 40.078;
atom["Sc"]= 44.955912;
atom["Ti"]= 47.867;
atom["V"]= 50.9415;
atom["Cr"]= 51.9961;
atom["Mn"]= 54.938045;
atom["Fe"]= 55.845;
atom["Co"]= 58.933195;
atom["Ni"]= 58.6934;
atom["Cu"]= 63.546;
atom["Zn"]= 65.38;
atom["Ga"]= 69.723;
atom["Ge"]= 72.64;
atom["As"]= 74.9216;
atom["Se"]= 78.96;
atom["Br"]= 79.904;
atom["Kr"]= 83.798;
atom["Rb"]= 85.4678;
atom["Sr"]= 87.62;
atom["Y"]= 88.90585;
atom["Zr"]= 91.224;
atom["Nb"]= 92.90638;
atom["Mo"]= 95.96;
atom["Tc"]= 98;
atom["Ru"]= 101.07;
atom["Rh"]= 102.9055;
atom["Pd"]= 106.42;
atom["Ag"]= 107.8682;
atom["Cd"]= 112.411;
atom["In"]= 114.818;
atom["Sn"]= 118.71;
atom["Sb"]= 121.76;
atom["Te"]= 127.6;
atom["I"]= 126.90447;
atom["Xe"]= 131.293;
atom["Cs"]= 132.9054519;
atom["Ba"]= 137.327;
atom["La"]= 138.90547;
atom["Ce"]= 140.116;
atom["Pr"]= 140.90765;
atom["Nd"]= 144.242;
atom["Pm"]= 145;
atom["Sm"]= 150.36;
atom["Eu"]= 151.964;
atom["Gd"]= 157.25;
atom["Tb"]= 158.92535;
atom["Dy"]= 162.5;
atom["Ho"]= 164.93032;
atom["Er"]= 167.259;
atom["Tm"]= 168.93421;
atom["Yb"]= 173.054;
atom["Lu"]= 174.9668;
atom["Hf"]= 178.49;
atom["Ta"]= 180.94788;
atom["W"]= 183.84;
atom["Re"]= 186.207;
atom["Os"]= 190.23;
atom["Ir"]= 192.217;
atom["Pt"]= 195.084;
atom["Au"]= 196.966569;
atom["Hg"]= 200.59;
atom["Tl"]= 204.3833;
atom["Pb"]= 207.2;
atom["Bi"]= 208.9804;
atom["Po"]= 209;
atom["At"]= 210;
atom["Rn"]= 222;
atom["Fr"]= 223;
atom["Ra"]= 226;
atom["Ac"]= 227;
atom["Th"]= 232.03806;
atom["Pa"]= 231.03588;
atom["U"]= 238.02891;
atom["Np"]= 237;
atom["Pu"]= 244;
atom["Am"]= 243;
atom["Cm"]= 247;
atom["Bk"]= 247;
atom["Cf"]= 251;
atom["Es"]= 252;
atom["Fm"]= 257;
atom["Md"]= 258;
atom["No"]= 259;
atom["Lr"]= 262;
atom["Rf"]= 267;
atom["Db"]= 268;
atom["Sg"]= 271;
atom["Bh"]= 272;
atom["Hs"]= 270;
atom["Mt"]= 276;
atom["Ds"]= 281;
atom["Rg"]= 280;
atom["Uub"]= 285;
atom["Uut"]= 284;
atom["Uuq"]= 289;
atom["Uup"]= 288;
atom["Uuh"]= 293;
atom["Uuo"]= 294;
uppercase="ABCDEFGHIJKLMNOPQRSTUVWXYZ";
lowercase="abcdefghijklmnopqrstuvwxyz";
number="0123456789.";
function calculate(formula) {
percision=document.forms[0].elements[1].value;
total=new Array(); level=0; total[0]=0; i=0; mass=0; name='';
elmass=new Array();
for (i=0; i<elmass.length;i++) {
elmass[i]=null;
}
elmass[0]=new Array();
for (i=0; i<elmass[0].length;i++) {
elmass[0][i]=0;
} i=0;
while (formula.charAt(i)!="") {
if ((uppercase+lowercase+number+"()").indexOf(formula.charAt(i))==-1) i++;
while (formula.charAt(i)=="(") {
level++;
i++;
total[level]=0;
elmass[level]=new Array();
for (h=0; i<elmass[level].length;h++) {
elmass[level][i]=0;
} }
if (formula.charAt(i)==")") {
mass=total[level];
name='';
level--;
}
else if (uppercase.indexOf(formula.charAt(i))!=-1) {
name=formula.charAt(i);
while (lowercase.indexOf(formula.charAt(i+1))!=-1 && formula.charAt(i+1)!="") name+=formula.charAt(++i);
mass=atom[name];
// massStr=mass+"";
// if (massStr.indexOf(".")!=-1)
// masspercis=(massStr.substring(massStr.indexOf(".")+1,massStr.length)).length;
// else // masspercis=0;
// percision=(percision==8 || percision>masspercis)?masspercis:percision;
}
var amount="";
while (number.indexOf(formula.charAt(i+1))!=-1 && formula.charAt(i+1)!="") amount+=formula.charAt(++i);
if (amount=="") amount="1";
total[level]+=mass*parseFloat(amount);
if (name=="") {
for (ele in elmass[level+1]) {
totalnumber=parseFloat(elmass[level+1][ele])*amount
if (elmass[level][ele]==null) elmass[level][ele]=totalnumber;
else
elmass[level][ele]=totalnumber+parseFloat(elmass[level][ele]);
}
}
else {
if (elmass[level][name]==null) elmass[level][name]=amount;
else elmass[level][name]=parseFloat(elmass[level][name])+parseFloat(amount);
}
i++;
}
weight=rounded(total[0],percision);
previous=document.forms[1].elements[0].value;
document.forms[1].elements[0].value=document.forms[0].elements[0].value+":"+newline();
for (ele in elmass[0]) {
eltotal=eval(elmass[0][ele]*atom[ele]);
document.forms[1].elements[0].value+=elmass[0][ele]+" "+ele+" * "+atom[ele]+" = "+rounded(eltotal,percision)+" ("+rounded(eltotal/total[0]*100,percision)+"% of mass)"+newline();
}
document.forms[1].elements[0].value+= "Total:"+weight+" g/mol"+newline();
document.forms[1].elements[0].value+="------------------------------------------------------------"+newline()+previous;
document.forms[0].elements[0].value='';
document.forms[0].elements[0].focus();
}
function newline() {
return (navigator.appName.substring(0,9)=="Microsoft")?'\r':'\n';
}
function rounded(number,init_percision)
{
var rounded=Math.round(number*Math.pow(10,init_percision))/Math.pow(10,init_percision);
var numStr=rounded+"";
var percis=(numStr.substring(numStr.indexOf(".")+1,numStr.length)).length;
if (numStr.indexOf(".")!=-1){
var extrazeros=(init_percision-percis<0)?0:init_percision-percis;
for (var i=0;i<extrazeros;i++){
rounded=rounded+"0";
}
}
return rounded;
}
function printpage() {
printwindow= window.open('','','menubar=yes,toolbar=yes,location=yes,directories=yes,status=yes,scrollbars=yes,resizeable=yes,copyhistory=no');
printwindow.document.clear();
printwindow.document.writeln("<html><head><title>Molecular Weight Results</title></head><body><pre><tt><code><kbd>");
printwindow.document.writeln(document.forms[1].elements[0].value);
printwindow.document.writeln("</kbd></code></tt></pre></body></html>");
printwindow.document.close();
}
</script>

<form action="javascript:" onsubmit="calculate(document.forms[0].elements[0].value)"><input size="35" type="text">&nbsp;&nbsp;&nbsp;&nbsp;Decimals?&nbsp;
<input size="2" value="3" type="text">&nbsp;<input value="Calculate" onclick="calculate(document.forms[0].elements[0].value)" type="button">
<input onclick="document.forms[1].elements[0].value=''" value="Clear" type="button"></form>
<form>
<textarea cols="60" rows="20"></textarea><br>

</form>

