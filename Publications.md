---
layout: page
title: Publications
---


<html>
<head>

<!--// cabecera de altmetrics -->
<script type='text/javascript' src='https://d1bxh8uas1mnw7.cloudfront.net/assets/embed.js'></script>
<script async src="https://badge.dimensions.ai/badge.js" charset="utf-8"></script>
<!--// cabecera de altmetrics -->

<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<script type="text/javascript">
<!--
// QuickSearch script for JabRef HTML export (no Abstract/BibTeX)
// Version: 3.0
//
// Copyright (c) 2006-2011, Mark Schenk
//
// This software is distributed under a Creative Commons Attribution 3.0 License
// http://creativecommons.org/licenses/by/3.0/
//
// Features:
// - intuitive find-as-you-type searching
//    ~ case insensitive
//    ~ ignore diacritics (optional)
//
// - search with/without Regular Expressions
// - match BibTeX key
//

// Search settings
var noSquiggles = true; 	// ignore diacritics when searching
var searchRegExp = false; 	// enable RegExp searches


if (window.addEventListener) {
	window.addEventListener("load",initSearch,false); }
else if (window.attachEvent) {
	window.attachEvent("onload", initSearch); }

function initSearch() {
	// check for quick search table and searchfield
	if (!document.getElementById('qs_table')||!document.getElementById('quicksearch')) { return; }

	// load all the rows and sort into arrays
	loadTableData();
	
	//find the query field
	qsfield = document.getElementById('qs_field');

	// previous search term; used for speed optimisation
	prevSearch = '';

	//find statistics location
	stats = document.getElementById('stat');
	setStatistics(-1);
	
	// set up preferences
	initPreferences();

	// shows the searchfield
	document.getElementById('quicksearch').style.display = 'block';
	document.getElementById('qs_field').onkeyup = quickSearch;
}

function loadTableData() {
	// find table and appropriate rows
	searchTable = document.getElementById('qs_table');
	var allRows = searchTable.getElementsByTagName('tbody')[0].getElementsByTagName('tr');

	// split all rows into entryRows and infoRows (e.g. abstract, review, bibtex)
	entryRows = new Array();

	// get data from each row
	entryRowsData = new Array();
	
	BibTeXKeys = new Array();
	
	for (var i=0, k=0, j=0; i<allRows.length;i++) {
		if (allRows[i].className.match(/entry/)) {
			entryRows[j] = allRows[i];
			entryRowsData[j] = stripDiacritics(getTextContent(allRows[i]));
			allRows[i].id ? BibTeXKeys[j] = allRows[i].id : allRows[i].id = 'autokey_'+j;
			j ++;
		}
	}
	//number of entries and rows
	numEntries = entryRows.length;
}

function quickSearch(){
	
	tInput = qsfield;

	if (tInput.value.length == 0) {
		showAll();
		setStatistics(-1);
		qsfield.className = '';
		return;
	} else {
		t = stripDiacritics(tInput.value);

		if(!searchRegExp) { t = escapeRegExp(t); }
			
		// only search for valid RegExp
		try {
			textRegExp = new RegExp(t,"i");
			qsfield.className = '';
		}
			catch(err) {
			prevSearch = tInput.value;
			qsfield.className = 'invalidsearch';
			return;
		}
	}
	
	// count number of hits
	var hits = 0;

	// start looping through all entry rows
	for (var i = 0; cRow = entryRows[i]; i++){

		// only show search the cells if it isn't already hidden OR if the search term is getting shorter, then search all
		if(cRow.className.indexOf('noshow')==-1 || tInput.value.length <= prevSearch.length){
			var found = false; 

			if (entryRowsData[i].search(textRegExp) != -1 || BibTeXKeys[i].search(textRegExp) != -1){ 
				found = true;
			}
			
			if (found){
				cRow.className = 'entry show';
				hits++;
			} else {
				cRow.className = 'entry noshow';
			}
		}
	}

	// update statistics
	setStatistics(hits)
	
	// set previous search value
	prevSearch = tInput.value;
}


// Strip Diacritics from text
// http://stackoverflow.com/questions/990904/javascript-remove-accents-in-strings

// String containing replacement characters for stripping accents 
var stripstring = 
    'AAAAAAACEEEEIIII'+
    'DNOOOOO.OUUUUY..'+
    'aaaaaaaceeeeiiii'+
    'dnooooo.ouuuuy.y'+
    'AaAaAaCcCcCcCcDd'+
    'DdEeEeEeEeEeGgGg'+
    'GgGgHhHhIiIiIiIi'+
    'IiIiJjKkkLlLlLlL'+
    'lJlNnNnNnnNnOoOo'+
    'OoOoRrRrRrSsSsSs'+
    'SsTtTtTtUuUuUuUu'+
    'UuUuWwYyYZzZzZz.';

function stripDiacritics(str){

    if(noSquiggles==false){
        return str;
    }

    var answer='';
    for(var i=0;i<str.length;i++){
        var ch=str[i];
        var chindex=ch.charCodeAt(0)-192;   // Index of character code in the strip string
        if(chindex>=0 && chindex<stripstring.length){
            // Character is within our table, so we can strip the accent...
            var outch=stripstring.charAt(chindex);
            // ...unless it was shown as a '.'
            if(outch!='.')ch=outch;
        }
        answer+=ch;
    }
    return answer;
}

// http://stackoverflow.com/questions/3446170/escape-string-for-use-in-javascript-regex
// NOTE: must escape every \ in the export code because of the JabRef Export...
function escapeRegExp(str) {
  return str.replace(/[-\[\]\/\{\}\(\)\*\+\?\.\\\^\$\|]/g, "\\$&");
}

function setStatistics (hits) {
	if(hits < 0) { hits=numEntries; }
	if(stats) { stats.firstChild.data = hits + '/' + numEntries}
}

function getTextContent(node) {
	// Function written by Arve Bersvendsen
	// http://www.virtuelvis.com
	
	if (node.nodeType == 3) {
	return node.nodeValue;
	} // text node
	if (node.nodeType == 1 && node.className != "infolinks") { // element node
	var text = [];
	for (var chld = node.firstChild;chld;chld=chld.nextSibling) {
		text.push(getTextContent(chld));
	}
	return text.join("");
	} return ""; // some other node, won't contain text nodes.
}

function showAll(){
	for (var i = 0; i < numEntries; i++){ entryRows[i].className = 'entry show'; }
}

function clearQS() {
	qsfield.value = '';
	showAll();
}

function redoQS(){
	showAll();
	quickSearch(qsfield);
}

function updateSetting(obj){
	var option = obj.id;
	var checked = obj.value;

	switch(option)
	 {
	 case "opt_useRegExp":
	   searchRegExp=!searchRegExp;
	   redoQS();
	   break;
	 case "opt_noAccents":
	   noSquiggles=!noSquiggles;
	   loadTableData();
	   redoQS();
	   break;
	 }
}

function initPreferences(){
	if(noSquiggles){document.getElementById("opt_noAccents").checked = true;}
	if(searchRegExp){document.getElementById("opt_useRegExp").checked = true;}
}

function toggleSettings(){
	var togglebutton = document.getElementById('showsettings');
	var settings = document.getElementById('settings');
	
	if(settings.className == "hidden"){
		settings.className = "show";
		togglebutton.innerText = "close settings";
		togglebutton.textContent = "close settings";
	}else{
		settings.className = "hidden";
		togglebutton.innerText = "settings...";		
		togglebutton.textContent = "settings...";
	}
}

-->
</script>

<style type="text/css">
body { background-color: white; font-family: "Helvetica Neue", Helvetica, Arial, sans-serif; line-height: 1.5; padding: 0em; color: #2E2E2E; margin: auto 0em; }

form#quicksearch { width: auto; border-style: solid; border-color: gray; border-width: 1px 0px; padding: 0.7em 0.5em; display:none; position:relative; }
span#searchstat {padding-left: 1em;}

div#settings { margin-top:0.7em; /* border-bottom: 1px transparent solid; background-color: #efefef; border: 1px grey solid; */ }
div#settings ul {margin: 0; padding: 0; }
div#settings li {margin: 0; padding: 0 1em 0 0; display: inline; list-style: none; }
div#settings li + li { border-left: 2px #efefef solid; padding-left: 0.5em;}
div#settings input { margin-bottom: 0px;}

div#settings.hidden {display:none;}

#showsettings { border: 1px grey solid; padding: 0 0.5em; float:right; line-height: 1.6em; text-align: right; }
#showsettings:hover { cursor: pointer; }

.invalidsearch { background-color: red; }
input[type="button"] { background-color: #efefef; border: 1px #2E2E2E solid;}

table { width: 100%; empty-cells: show; border-spacing: 0em 0.2em; margin: 1em 0em; border-style: none; }
th, td { border: 1px gray solid; border-width: 1px 1px; padding: 0.5em; vertical-align: top; text-align: left; }
th { background-color: #efefef; }
td + td, th + th { border-left: none; }

td a { color: navy; text-decoration: none; }
td a:hover  { text-decoration: underline; }

tr.noshow { display: none;}
tr.highlight td { background-color: #EFEFEF; border-top: 2px #2E2E2E solid; font-weight: bold; }
tr.abstract td, tr.review td, tr.bibtex td { background-color: #EFEFEF; text-align: justify; border-bottom: 2px #2E2E2E solid; }
tr.nextshow td { border-bottom: 1px gray solid; }

tr.bibtex pre { width: 100%; overflow: auto; white-space: pre-wrap;}
p.infolinks { margin: 0.3em 0em 0em 0em; padding: 0px; }

@media print {
	p.infolinks, #qs_settings, #quicksearch, t.bibtex { display: none !important; }
	tr { page-break-inside: avoid; }
}
</style>




</head>








<body >


<form action="" id="quicksearch">
<input type="text" id="qs_field" autocomplete="off" placeholder="Type to search..." /> <input type="button" onclick="clearQS()" value="clear" />
<span id="searchstat">Matching entries: <span id="stat">0</span></span>
<div id="showsettings" onclick="toggleSettings()">settings...</div>
<div id="settings" class="hidden">
<ul>
<li><input type="checkbox" class="search_setting" id="opt_useRegExp" onchange="updateSetting(this)"><label for="opt_useRegExp"> use RegExp</label></li>
<li><input type="checkbox" class="search_setting" id="opt_noAccents" onchange="updateSetting(this)"><label for="opt_noAccents"> ignore accents</label></li>
</ul>
</div>
</form>




<table id="qs_table" border="1" font-size="10">
<thead><tr><th width="20%">Author</th><th width="30%">Title</th><th width="5%">Year</th><th width="30%">Journal/Proceedings</th><th width="10%">DOI/URL</th><th width="5%">Altmetrics</th></tr></thead>





<tbody>
<tr id="Jacobsson2021open1" class="entry">
	<td>Jacobsson, T.J. et al.</td>
	<td>An open-access database and analysis tool for perovskite solar cells based on the FAIR data principles</td>
	<td>2021</td>
	<td>Nature Energy, pp. 1-9&nbsp;</td>
	<td>article <a href="https://doi.org/10.1038/s41560-021-00941-3" >DOI</a> &nbsp;</td> 
<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1038/s41560-021-00941-3" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1038/s41560-021-00941-3" > </span></td>

</tr>



	
	<tr id="Garcia-Fernandez2021Perovskites143" class="entry">
	<td>García-Fernández, A., Juárez-Pérez, E.J., Castro-García, S., Sánchez-Andújar, M. and Señaris-Rodríguez, M.A.</td>
	<td>Perovskites And Other Framework Structure Crystalline Materials. New Trends and Perspectives. Chap. 4. Hybrid organic-inorganic perovskites: a spin-off of oxidic perovskites</td>
	<td>2021</td>
	<td>OAJ Materials and Devices<br/>Vol. 5(1), pp. 143-174&nbsp;</td>
	<td>inbook <a href="https://books.google.es/books?id=DHwUEAAAQBAJ&lpg=PP1&pg=PA143#v=twopage&q&f=false">URL</a>  <a href="http://perovskitesandotherfws.co-ac.com/"> Publisher</a></td>
	<td>&nbsp;</td>
</tr>
<tr id="Haro2021Nano16" class="entry">
	<td>Haro, M., Kumar, P., Zhao, J., Koutsogiannis, P., Porkovich, A.J., Ziadi, Z., Bouloumis, T., Singh, V., Juarez-Perez, E.J., Toulkeridou, E., Nordlund, K., Djurabekova, F., Sowwan, M. and Grammatikopoulos, P.</td>
	<td>Nano-vault architecture mitigates stress in silicon-based anodes for lithium-ion batteries</td>
	<td>2021</td>
	<td>Communications Materials<br/>Vol. 2(1), pp. 16-&nbsp;</td>
	<td>article<a href="http://dx.doi.org/10.1038/s43246-021-00119-0"> DOI</a> &nbsp;</td>
	<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1038/s43246-021-00119-0" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1038/s43246-021-00119-0" > </span></td>

</tr>
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	<tr id="Juarez-Perez2020Perovskite1309" class="entry">
	<td>Juarez-Perez, E.J.* and Haro, M.</td>
	<td>Perovskite solar cells take a step forward</td>
	<td>2020</td>
	<td>Science<br/>Vol. 368(6497), pp. 1309-1309&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1126/science.abc5401">DOI</a> <a href="https://science.sciencemag.org/content/sci/368/6497/1309.full.pdf?ijkey=AiGqI3IVgpjXM&keytype=ref&siteid=sci">Reprint</a></td>
	<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1126/science.abc5401" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1126/science.abc5401" > </span></td>
	</tr>

<tr id="Suarez2020Mechanisms64071" class="entry">
	<td>Suarez, I., Juarez-Perez, E., Chirvony, V., Mora-Sero, I. and Martinez-Pastor, J.</td>
	<td>Mechanisms of Spontaneous and Amplified Spontaneous Emission in CH<sub>3</sub>NH<sub>3</sub>PbI<sub>3</sub> Perovskite Thin Films Integrated in an Optical Waveguide</td>
	<td>2020</td>
	<td>Phys. Rev. Applied<br/>Vol. 13, pp. 064071&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1103/PhysRevApplied.13.064071">DOI</a> &nbsp;</td>
	<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1103/PhysRevApplied.13.064071" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1103/PhysRevApplied.13.064071" > </span></td>
</tr>




<tr id="Zhidkov2020Influence0" class="entry">
	<td>Zhidkov, I.S., Boukhvalov, D.W., Kukharenko, A.I., Finkelstein, L.D., Cholakh, S.O., Akbulatov, A.F., Juarez-Perez, E.J., Troshin, P.A. and Kurmaev, E.Z.</td>
	<td>Influence of Ion Migration from ITO and SiO<sub>2</sub> Substrates on Photo and Thermal Stability of CH<sub>3</sub>NH<sub>3</sub>SnI<sub>3</sub> Hybrid Perovskite</td>
	<td>2020</td>
	<td>J. Phys. Chem. C<br/>Vol. 124, pp. 14928-14934</td>
	<td>article <a href="http://dx.doi.org/10.1021/acs.jpcc.0c04621">DOI</a> &nbsp;</td>
	<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/acs.jpcc.0c04621" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/acs.jpcc.0c04621" > </span></td>
</tr>


<tr id="Wang2020Approaching44604" class="entry">
	<td>Wang, Q., Juarez-Perez, E.J., Jiang, S., Xiao, M., Qian, J., Shin, E.-Y., Noh, Y.-Y., Qi, Y., Shi, Y. and Li, Y.</td>
	<td>Approaching isotropic transfer integrals in crystalline organic semiconductors</td>
	<td>2020</td>
	<td>Physical Review Materials<br/>Vol. 4(4), pp. 044604&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1103/PhysRevMaterials.4.044604">DOI</a> &nbsp;</td>
	<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1103/PhysRevMaterials.4.044604" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1103/PhysRevMaterials.4.044604" > </span></td>
</tr>




	
	
	<tr id="Chulia-Jordan2019Inhibition22378" class="entry">
	<td>Chuli&aacute;-Jord&aacute;n, R., Fern&aacute;ndez-Delgado, N., Ju&aacute;rez-P&eacute;rez, E.J., Mora-Ser&oacute;, I., Herrera, M., Molina, S.I. and Mart&iacute;nez-Pastor, J.P.</td>
	<td>Inhibition of light emission from the metastable tetragonal phase at low temperatures in island-like films of lead iodide perovskites.</td>
	<td>2019</td>
	<td>Nanoscale<br/>Vol. 11, pp. 22378-22386&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/c9nr07543g">DOI</a> &nbsp;</td>
	<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/c9nr07543g" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/c9nr07543g" > </span></td>
	
</tr>





<tr id="Santos2019Nanoestructuras97" class="entry">
	<td>Santos, E.V., Zamora, F.F., Ram&iacute;rez, B.G., Snyders, R., Noirfalise, X., Guisbiers, G., Alem&aacute;n, K.P., Ju&aacute;rez-P&eacute;rez, E.J., Saint-Gr&eacute;goire, P., Vaillant, L., Azze, M.G., C&aacute;rdenas, A.A.B., Gil, A.S., L&oacute;pez, C.L., Le&oacute;n, R.T. and Hern&aacute;ndez, S.S.</td>
	<td>Nanoestructuras semiconductoras en base a &oacute;xido c&uacute;prico y di&oacute;xido de titanio: S&iacute;ntesis, caracterizaci&oacute;n y funcionamiento como fotoelectrodos</td>
	<td>2019</td>
	<td>Anales de la Academia de Ciencias de Cuba<br/>Vol. 9(3), pp. 97-101&nbsp;</td>
	<td>article <a href="http://www.revistaccuba.sld.cu/index.php/revacc/article/view/668">URL</a>&nbsp;</td>
	<td></td>
</tr>
	
	
	
	
<tr id="Bisquert2019Causes5889" class="entry">
	<td>Bisquert, J. and Juarez-Perez, E.J.*</td>
	<td>The Causes of Degradation of Perovskite Solar Cells</td>
	<td>2019</td>
	<td>J. Phys. Chem. Lett.<br/>Vol. 10, pp. 5889-5891&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/acs.jpclett.9b00613">DOI</a> &nbsp;</td>
	<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/acs.jpclett.9b00613" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/acs.jpclett.9b00613" > </span></td>
</tr>
	
	

<tr id="He2019Carbon203" class="entry">
	<td>He, S., Qiu, L., Son, D.-Y., Liu, Z., Juarez-Perez, E.J., Ono, L.K., Stecker, C. and Qi, Y.</td>
	<td>Carbon-based Electrode Engineering Boosts the Efficiency of All Low-temperature Processed Perovskite Solar Cells</td>
	<td>2019</td>
	<td>ACS Energy Letters<br/>Vol. 4, pp. 2032-2039&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/acsenergylett.9b01294">DOI</a> &nbsp;</td>
	<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/acsenergylett.9b01294" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/acsenergylett.9b01294" > </span></td>
</tr>



<tr id="Garcia-Fernandez2019Hybrid10008" class="entry">
	<td>Garc&iacute;a-Fern&aacute;ndez, A., Juarez-Perez, E.J., Berm&uacute;dez-Garc&iacute;a, J.M., Llamas-Saiz, A.L., Artiaga, R., L&oacute;pez-Beceiro, J.J., Se&ntilde;ar&iacute;s-Rodr&iacute;guez, M.A., S&aacute;nchez-And&uacute;jar, M. and Castro-Garc&iacute;a, S.</td>
	<td>Hybrid lead halide [(CH<sub>3</sub>)<sub>2</sub>NH<sub>2</sub>]PbX<sub>3</sub> (X = Cl<sup>-</sup> and Br<sup>-</sup>) hexagonal perovskites with multiple functional properties</td>
	<td>2019</td>
	<td>J. Mater. Chem. C<br/>Vol. 7, pp. 10008-10018&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/C9TC03543E">DOI</a> &nbsp;</td>
	<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/C9TC03543E" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/C9TC03543E" > </span></td>
</tr>






<tr id="Juarez-Perez2019Thermal16912" class="entry">
	<td>Juarez-Perez, E.J.,* Ono, L.K. and Qi, Y.</td>
	<td>Thermal degradation of formamidinium based lead halide perovskites into sym-triazine and hydrogen cyanide observed by coupled thermogravimetry - mass spectrometry analysis</td>
	<td>2019</td>
	<td>J. Mater. Chem. A<br/>Vol. 7, pp. 16912-16919&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/C9TA06058H">DOI</a> &nbsp;</td>
<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/C9TA06058H" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/C9TA06058H" > </span></td>


</tr>
<tr id="Jiang2019Reduction585" class="entry">
	<td>Jiang, Y., Qiu, L., Juarez-Perez, E.J., Ono, L.K., Hu, Z., Liu, Z., Wu, Z., Meng, L., Wang, Q. and Qi, Y.</td>
	<td>Reduction of lead leakage from damaged lead halide perovskite solar modules using self-healing polymer-based encapsulation</td>
	<td>2019</td>
	<td>Nature Energy<br/>Vol. 4, pp. 585-593&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1038/s41560-019-0406-2">DOI</a> &nbsp;</td>
<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1038/s41560-019-0406-2" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1038/s41560-019-0406-2" > </span></td>





</tr>

<tr id="Juarez-Perez2019Determination1900126" class="entry">
	<td>Juarez-Perez, E.J.* and Qi, Y.</td>
	<td>Determination of Carrier Diffusion Length Using Transient Electron Photoemission Microscopy in the GaAs/InSe Heterojunction</td>
	<td>2019</td>
	<td>Phys. Status Solidi B </br> pp. 1900126&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/pssb.201900126">DOI</a> &nbsp;</td>
	<td><div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/pssb.201900126" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/pssb.201900126" > </span></td>
</tr>



<tr id="Juarez-Perez2019Comment" class="entry">
	<td>Juarez-Perez, E.J.*</td>
	<td>Comment on “Probing the Origins of Photodegradation in OrganicInorganic Metal Halide Perovskites with Time-Resolved Mass Spectrometry”, Sustainable Energy &amp; Fuels, 2018. Updated response (March 14, 2019)</td>
	<td>2019</td>
	<td>ChemRxiv&nbsp;</td>
	<td>preprint <a href="http://dx.doi.org/10.26434/chemrxiv.7295585">DOI</a> &nbsp;</td>  
	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.26434/chemrxiv.7295585" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.26434/chemrxiv.7295585" > </span></td>
</tr>
<tr id="Juarez-Perez2019Degradation12586" class="entry">
	<td>Juarez-Perez, E.J.,* Ono, L.K., Uriarte, I., Cocinero, E.J. and Qi, Y.</td>
	<td>Degradation Mechanism and Relative Stability of Methylammonium Halide Based Perovskites Analyzed on the Basis of Acid-Base Theory</td>
	<td>2019</td>
	<td>ACS Appl. Mater. Interfaces<br/>Vol. 11, pp. 12586-12593&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/acsami.9b02374">DOI</a> &nbsp;</td>
<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/acsami.9b02374" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/acsami.9b02374" > </span></td>

</tr>
<tr id="Fernandez-Delgado2019Structural135701" class="entry">
	<td>Fernández-Delgado, N., Herrera, M., Delgado, F.J., Tavabi, A.H., Luysberg, M., DuninBorkowski, R.E., Juárez-Pérez, E.J., Hames, B.C., Mora-Sero, I., Suárez, I., Martínez-Pastor, J.P. and Molina, S.I.</td>
	<td>Structural characterization of bulk and nanoparticle lead halide perovskite thin films by (S)TEM techniques</td>
	<td>2019</td>
	<td>Nanotechnology<br/>Vol. 29, pp. 135701&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1088/1361-6528/aafc85">DOI</a> &nbsp;</td>
<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1088/1361-6528/aafc85" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1088/1361-6528/aafc85" > </span></td>

</tr>
<tr id="Jiang2019Negligible1803047" class="entry">
	<td>Jiang, Y., Remeika, M., Hu, Z., Juarez-Perez, E.J., Qiu, L., Liu, Z., Kim, T., Ono, L.K., Son, D.-Y., Hawash, Z., Leyden, M.R., Wu, Z., Meng, L., Hu, J. and Qi, Y.</td>
	<td>Negligible-Pb-Waste and Upscalable Perovskite Deposition Technology for High-Operational-Stability Perovskite Solar Modules</td>
	<td>2019</td>
	<td>Adv. Energy Mater., pp. 1803047&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/aenm.201803047">DOI</a> &nbsp;</td>

<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/aenm.201803047" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/aenm.201803047" > </span></td>

</tr>
<tr id="Liu2018Gas3880" class="entry">
	<td>Liu, Z., Qiu, L., Juarez-Perez, E.J., Hawash, Z., Kim, T., Jiang, Y., Wu, Z., Raga, S.R., Ono, L.K., Liu, S.F. and Qi, Y.</td>
	<td>Gas-solid reaction based over one-micrometer thick stable perovskite films for efficient solar cells and modules.</td>
	<td>2018</td>
	<td>Nat. Commun.<br/>Vol. 9(1), pp. 3880&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1038/s41467-018-06317-8">DOI</a> &nbsp;</td>
	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1038/s41467-018-06317-8" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1038/s41467-018-06317-8" > </span></td>
</tr>
<tr id="Garcia-Fernandez2018Benchmarking1800242" class="entry">
	<td>Garcia-Fern&aacute;ndez, A., Juarez-Perez, E.J.,* Castro-Garcia, S., S&aacute;nchez-And&uacute;jar, M., Ono, L.K., Jiang, Y. and Qi, Y.</td>
	<td>Benchmarking chemical stability of arbitrarily mixed 3D hybrid halide perovskites for solar cell applications</td>
	<td>2018</td>
	<td>Small Methods, pp. 1800242&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/smtd.201800242" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/smtd.201800242" ></div>
 	<span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/smtd.201800242" > </span></td>
</tr>
<tr id="Padron2018Nanostructured386" class="entry">
	<td>Padrón, K., Juárez-Pérez, E.J., Forcade, F., Snyders, R., Noirfalise, X., Laza, C., Jiménez, J. and Vigil, E.</td>
	<td>Nanostructured CuO films deposited on fluorine doped tin oxide conducting glass with a facile technology</td>
	<td>2018</td>
	<td>Thin Solid Films<br/>Vol. 660, pp. 386-390&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1016/j.tsf.2018.06.035" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1016/j.tsf.2018.06.035" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1016/j.tsf.2018.06.035" > </span></td>
</tr>
<tr id="Ono2018influence294001" class="entry">
	<td>Ono, L.K., Hawash, Z., Juarez-Perez, E.J., Qiu, L., Jiang, Y. and Qi, Y.</td>
	<td>The influence of secondary solvents on the morphology of spiro-MeOTAD hole transport layer for lead halide perovskite solar cells</td>
	<td>2018</td>
	<td>J. Phys. D: Appl. Phys.<br/>Vol. 51, pp. 294001&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1088/1361-6463/aacb6e" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1088/1361-6463/aacb6e" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1088/1361-6463/aacb6e" > </span></td>
</tr>
<tr id="Juarez-Perez2018Photodecomposition9604" class="entry">
	<td>Juarez-Perez, E.J., Ono, L.K., Maeda, M., Jiang, Y., Hawash, Z. and Qi, Y.</td>
	<td>Photodecomposition and thermal decomposition in methylammonium halide lead perovskites and inferred design principles to increase photovoltaic device stability</td>
	<td>2018</td>
	<td>J. Mater. Chem. A<br/>Vol. 6, pp. 9604-9612&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/C8TA03501F" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/C8TA03501F" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/C8TA03501F" > </span></td>
</tr>
<tr id="Wang2018Spin1318" class="entry">
	<td>Wang, Q., Juarez-Perez, E.J., Jiang, S., Qiu, L., Ono, L.K., Sasaki, T., Wang, X., Shi, Y., Zheng, Y., Qi, Y. and Li, Y.</td>
	<td>Spin-Coated Crystalline Molecular Monolayers for Performance Enhancement in Organic Field-Effect Transistors</td>
	<td>2018</td>
	<td>J. Phys. Chem. Lett.<br/>Vol. 9, pp. 1318-1323&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/acs.jpclett.8b00352" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/acs.jpclett.8b00352" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/acs.jpclett.8b00352" > </span></td>
</tr>
<tr id="Jiang2018Combination1703835" class="entry">
	<td>Jiang, Y., Leyden, M.R., Qiu, L., Wang, S., Ono, L.K., Wu, Z., Juarez-Perez, E.J. and Qi, Y.</td>
	<td>Combination of Hybrid CVD and Cation Exchange for Upscaling Cs-Substituted Mixed Cation Perovskite Solar Cells with High Efficiency and Stability</td>
	<td>2018</td>
	<td>Adv. Funct. Mater.<br/>Vol. 28, pp. 1703835&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/adfm.201703835" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/adfm.201703835" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/adfm.201703835" > </span></td>
</tr>
<tr id="Wu2018Improved1703670" class="entry">
	<td>Wu, Z., Raga, S.R., Juarez-Perez, E.J., Yao, X., Jiang, Y., Ono, L.K., Ning, Z., Tian, H. and Qi, Y.B.</td>
	<td>Improved Efficiency and Stability of Perovskite Solar Cells Induced by C=O Functionalized Hydrophobic Ammonium-Based Additives</td>
	<td>2018</td>
	<td>Adv. Mater.<br/>Vol. 30, pp. 1703670&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/adma.201703670" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/adma.201703670" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/adma.201703670" > </span></td>
</tr>
<tr id="Alberola-Borras2018Relative169" class="entry">
	<td>Alberola-Borràs, J.-A., Vidal, R., Juárez-Pérez, E.J., Mas-Marzá, E., Guerrero, A. and Mora-Seró, I.</td>
	<td>Relative impacts of methylammonium lead triiodide perovskite solar cells based on life cycle assessment</td>
	<td>2018</td>
	<td>Sol. Energy Mater. Sol. Cells<br/>Vol. 179, pp. 169-177&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1016/j.solmat.2017.11.008" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1016/j.solmat.2017.11.008" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1016/j.solmat.2017.11.008" > </span></td>
</tr>
<tr id="Bisquert2017Hybrid" class="entry">
	<td>Bisquert, J., Juarez-Perez, E.J. and Kamat, P.V.</td>
	<td>Hybrid Perovskite Solar Cells. The Genesis and Early Developments, 2009-2014</td>
	<td>2017</td>
	<td>&nbsp;</td>
	<td>book <a href="http://www.worldcat.org/oclc/1100415744">URL</a>&nbsp;</td>
<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-isbn='978-84-947758-0-2'  ></div> </td>

</tr>
<tr id="Ono2017Progress30197" class="entry">
	<td>Ono, L.K., Juarez-Perez, E.J. and Qi, Y.</td>
	<td>Progress on Perovskite Materials and Solar Cells with Mixed Cations and Halide Anions</td>
	<td>2017</td>
	<td>ACS Appl. Mater. Interfaces<br/>Vol. 9, pp. 30197-30246&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/acsami.7b06001" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/acsami.7b06001" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/acsami.7b06001" > </span></td>
</tr>
<tr id="Leyden2017Methylammonium3193" class="entry">
	<td>Leyden, M., Meng, L., Jiang, Y., Ono, L., Qiu, L., Juarez-Perez, E., Qin, C., Adachi, C. and Qi, Y.</td>
	<td>Methylammonium Lead Bromide Perovskite Light-Emitting Diodes by Chemical Vapor Deposition</td>
	<td>2017</td>
	<td>J. Phys. Chem. Lett.<br/>Vol. 8, pp. 3193-3198&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/acs.jpclett.7b01093" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/acs.jpclett.7b01093" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/acs.jpclett.7b01093" > </span></td>
</tr>
<tr id="Suarez2017Optimization1" class="entry">
	<td>Suarez, I., Ngo, T.T., Juarez-Perez, E., Antonicelli, G., Cortizo-Lacalle, D., Mateo-Alonso, A., Mora-Sero, I. and Martinez-Pastor, J.</td>
	<td>Optimization of semiconductor halide perovskite layers to implement waveguide amplifiers</td>
	<td>2017</td>
	<td>2017 19th International Conference on Transparent Optical Networks (ICTON), pp. 1-3&nbsp;</td>
	<td>conference <a href="http://dx.doi.org/10.1109/ICTON.2017.8024988" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1109/ICTON.2017.8024988" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1109/ICTON.2017.8024988" > </span></td>
</tr>
<tr id="Ferrer-Ugalde2017Carborane2091" class="entry">
	<td>Ferrer-Ugalde, A., Cabrera-Gonz&aacute;lez, J., Ju&aacute;rez-P&eacute;rez, E.J., Teixidor, F., Perez-Inestrosa, E., Montenegro, J.M., Sillanp&auml;&auml;, R., Haukka, M. and Nunez, R.</td>
	<td>Carborane-stilbene dyads: the influence of substituents and cluster isomers on photoluminescence properties</td>
	<td>2017</td>
	<td>Dalton Trans.<br/>Vol. 46, pp. 2091-2104&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/c6dt04003a" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/c6dt04003a" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/c6dt04003a" > </span></td>
</tr>
<tr id="Wang2017Accelerated16195" class="entry">
	<td>Wang, S., Jiang, Y., Juarez-Perez, E.J., Ono, L.K. and Qi, Y.</td>
	<td>Accelerated degradation of methylammonium lead iodide perovskites induced by exposure to iodine vapour</td>
	<td>2017</td>
	<td>Nat. Energy<br/>Vol. 2, pp. 16195&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1038/nenergy.2016.195" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1038/nenergy.2016.195" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1038/nenergy.2016.195" > </span></td>
</tr>
<tr id="Climent-Pascual2016Influence18153" class="entry">
	<td>Climent-Pascual, E., Hames, B.C., Moreno-Ramírez, J.S., Álvarez, A.L., Juarez-Perez, E.J., Mas-Marza, E., Mora-Seró, I., de Andrés, A. and Coya, C.</td>
	<td>Influence of the substrate on the bulk properties of hybrid lead halide perovskite films</td>
	<td>2016</td>
	<td>J. Mater. Chem. A<br/>Vol. 4, pp. 18153-18163&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/c6ta08695k" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/c6ta08695k" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/c6ta08695k" > </span></td>
</tr>
<tr id="Fernandez-Delgado2016Structural604" class="entry">
	<td>Fernández-Delgado, N., Herrera-Collado, M., Delgado-González, F., Juárez-Pérez, E., Mora-Sero, I., Suárez, I., Martínez-Pastor, J. and Molina, S.</td>
	<td>Structural quality of CH<sub>3</sub>NH<sub>3</sub>PbI<sub>3</sub> perovskites for photovoltaic applications analyzed by electron microscopy techniques</td>
	<td>2016</td>
	<td>European Microscopy Congress 2016: Proceedings, pp. 604-605&nbsp;</td>
	<td>conference <a href="http://dx.doi.org/10.1002/9783527808465.emc2016.6019" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/9783527808465.emc2016.6019" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/9783527808465.emc2016.6019" > </span></td>
</tr>
<tr id="Suarez2016Halide" class="entry">
	<td>Suárez, I., Juárez-Pérez, E., Mora-Seró, I., Bisquert, J. and Martínez-Pastor, J.</td>
	<td>Halide perovskite amplifiers integrated in polymer waveguides</td>
	<td>2016</td>
	<td>International Conference on Transparent Optical Networks&nbsp;</td>
	<td>conference <a href="http://dx.doi.org/10.1109/ICTON.2016.7550528" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1109/ICTON.2016.7550528" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1109/ICTON.2016.7550528" > </span></td>
</tr>
<tr id="Juarez-Perez2016Thermal3406" class="entry">
	<td>Juarez-Perez, E.J., Hawash, Z., R. Raga, S., Ono, L.K. and Qi, Y.</td>
	<td>Thermal degradation of CH<sub>3</sub>NH<sub>3</sub>PbI<sub>3</sub> perovskite into NH<sub>3</sub> and CH<sub>3</sub>I gases observed by coupled thermogravimetry - mass spectrometry analysis</td>
	<td>2016</td>
	<td>Energy Environ. Sci.<br/>Vol. 9, pp. 3406-3410&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/C6EE02016J" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/C6EE02016J" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/C6EE02016J" > </span></td>
</tr>
<tr id="Jiang2016Post548" class="entry">
	<td>Jiang, Y., Juarez-Perez, E.J., Ge, Q., Wang, S., Leyden, M.R., Ono, L.K., Raga, S.R., Hu, J. and Qi, Y.</td>
	<td>Post-annealing of MAPbI<sub>3</sub> perovskite films with methylamine for efficient perovskite solar cells</td>
	<td>2016</td>
	<td>Mater. Horiz.<br/>Vol. 3, pp. 548-555&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/c6mh00160b" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/c6mh00160b" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/c6mh00160b" > </span></td>
</tr>
<tr id="Juarez-Perez2016Role5702" class="entry">
	<td>Juárez-Pérez, E.J., Leyden, M.R., Wang, S., Ono, L.K., Hawash, Z. and Qi, Y.</td>
	<td>Role of the Dopants on the Morphological and Transport Properties of Spiro-MeOTAD Hole Transport Layer</td>
	<td>2016</td>
	<td>Chem. Mater.<br/>Vol. 28, pp. 5702-5709&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/acs.chemmater.6b01777" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/acs.chemmater.6b01777" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/acs.chemmater.6b01777" > </span></td>
</tr>
<tr id="Jaramillo-Quintero2016Recombination6271" class="entry">
	<td>Jaramillo-Quintero, O.A., Solis de la Fuente, M., Sanchez, R.S., Recalde, I.B., Juarez-Perez, E.J., Rincon, M.E. and Mora-Sero, I.</td>
	<td>Recombination reduction on lead halide perovskite solar cells based on low temperature synthesized hierarchical TiO<sub>2</sub> nanorods</td>
	<td>2016</td>
	<td>Nanoscale<br/>Vol. 8, pp. 6271-6277&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/c5nr06692a" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/c5nr06692a" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/c5nr06692a" > </span></td>
</tr>
<tr id="Suarez2015PolymerPerovskite6157" class="entry">
	<td>Suarez, I., Juarez-Perez, E.J., Bisquert, J., Mora-Sero, I. and Martinez-Pastor, J.P.</td>
	<td>Polymer/Perovskite Amplifying Waveguides for Active Hybrid Silicon Photonics</td>
	<td>2015</td>
	<td>Adv. Mater.<br/>Vol. 27, pp. 6157-6162&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/adma.201503245" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/adma.201503245" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/adma.201503245" > </span></td>
</tr>
<tr id="Listorti2015Effect1628" class="entry">
	<td>Listorti, A., Juarez-Perez, E.J., Frontera, C., Roiati, V., Garcia-Andrade, L., Colella, S., Rizzo, A., Ortiz, P. and Mora-Sero, I.</td>
	<td>Effect of Mesostructured Layer upon Crystalline Properties and Device Performance on Perovskite Solar Cells</td>
	<td>2015</td>
	<td>J. Phys. Chem. Lett.<br/>Vol. 6, pp. 1628-1637&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/acs.jpclett.5b00483" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/acs.jpclett.5b00483" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/acs.jpclett.5b00483" > </span></td>
</tr>
<tr id="Zhang2015Fast4909" class="entry">
	<td>Zhang, J., Juarez-Perez, E.J., Mora-Sero, I., Viana, B. and Pauporte, T.</td>
	<td>Fast and low temperature growth of electron transport layers for efficient perovskite solar cells</td>
	<td>2015</td>
	<td>J. Mater. Chem. A<br/>Vol. 3, pp. 4909-4915&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/C4TA06416J" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/C4TA06416J" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/C4TA06416J" > </span></td>
</tr>
<tr id="Paez2015Fast2" class="entry">
	<td>Paez, E.I., Haro, M., Juarez-Perez, E.J., Carmona, R.J., Parra, J.B., Ramos, R.L. and Ania, C.O.</td>
	<td>Fast synthesis of micro/mesoporous xerogels: Textural and energetic assessment</td>
	<td>2015</td>
	<td>Microporous Mesoporous Mater.<br/>Vol. 209(0), pp. 2-9&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1016/j.micromeso.2014.10.033" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1016/j.micromeso.2014.10.033" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1016/j.micromeso.2014.10.033" > </span></td>
</tr>
<tr id="Guerrero2014Electrical133902" class="entry">
	<td>Guerrero, A., Juarez-Perez, E.J., Bisquert, J., Mora-Sero, I. and Garcia-Belmonte, G.</td>
	<td>Electrical field profile and doping in planar lead halide perovskite solar cells</td>
	<td>2014</td>
	<td>Appl. Phys. Lett.<br/>Vol. 105(13), pp. 133902&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1063/1.4896779" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1063/1.4896779" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1063/1.4896779" > </span></td>
</tr>
<tr id="Sudhagar2014Low89" class="entry">
	<td>Sudhagar, P., Juarez-Perez, E.J., Kang, Y. and Mora-Sero, I.</td>
	<td>Low-cost Nanomaterials. Chapter: Quantum Dot-Sensitized Solar Cells</td>
	<td>2014</td>
	<td>pp. 89-136&nbsp;</td>
	<td>inbook <a href="http://dx.doi.org/10.1007/978-1-4471-6473-9_5" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1007/978-1-4471-6473-9_5" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1007/978-1-4471-6473-9_5" > </span></td>
</tr>
<tr id="Juarez-Perez2014Photoinduced2390" class="entry">
	<td>Juarez-Perez, E.J., Sanchez, R.S., Badia, L., Garcia-Belmonte, G., Kang, Y.S., Mora-Sero, I. and Bisquert, J.</td>
	<td>Photoinduced Giant Dielectric Constant in Lead Halide Perovskite Solar Cells</td>
	<td>2014</td>
	<td>J. Phys. Chem. Lett.<br/>Vol. 5(0), pp. 2390-2394&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/jz5011169" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/jz5011169" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/jz5011169" > </span></td>
</tr>
<tr id="Ruisanchez2014Microwave10" class="entry">
	<td>Ruisanchez, E., Juarez-Perez, E.J., Arenillas, A., Bermudez, J.M. and Menendez, J.A.</td>
	<td>Microwave-assisted grinding of metallurgical coke</td>
	<td>2014</td>
	<td>Rev. Metal.<br/>Vol. 50(2), pp. e013;</td>
	<td>article <a href="http://dx.doi.org/10.3989/revmetalm.013" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.3989/revmetalm.013" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.3989/revmetalm.013" > </span></td>
</tr>
<tr id="Gonzalez-Pedro2014General888" class="entry">
	<td>Gonzalez-Pedro, V., Juarez-Perez, E.J., Arsyad, W.-S., Barea, E.M., Fabregat-Santiago, F., Mora-Sero, I. and Bisquert, J.</td>
	<td>General Working Principles of CH<sub>3</sub>NH<sub>3</sub>PbX<sub>3</sub> Perovskite Solar Cells</td>
	<td>2014</td>
	<td>Nano Lett.<br/>Vol. 14, pp. 888-893&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/nl404252e" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/nl404252e" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/nl404252e" > </span></td>
</tr>
<tr id="Juarez-Perez2014Role680" class="entry">
	<td>Juarez-Perez, E.J., Wussler, M., Fabregat-Santiago, F., Lakus-Wollny, K., Mankel, E., Mayer, T., Jaegermann, W. and Mora-Sero, I.</td>
	<td>Role of the Selective Contacts in the Performance of Lead Halide Perovskite Solar Cells</td>
	<td>2014</td>
	<td>J. Phys. Chem. Lett.<br/>Vol. 5(ja), pp. 680-685&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/jz500059v" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/jz500059v" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/jz500059v" > </span></td>
</tr>
<tr id="Pop2014OrganoseleniumII2221" class="entry">
	<td>Pop, A., Silvestru, A., Juarez-Perez, E.J., Arca, M., Lippolis, V. and Silvestru, C.</td>
	<td>Organoselenium(II) halides containing the pincer 2,6-(Me<sub>2</sub>NCH<sub>2</sub>)<sub>2</sub>C<sub>6</sub>H<sub>3</sub> ligand - an experimental and theoretical investigation</td>
	<td>2014</td>
	<td>Dalton Trans.<br/>Vol. 43, pp. 2221-2233&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/c3dt52886c" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/c3dt52886c" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/c3dt52886c" > </span></td>
</tr>
<tr id="Ferrer-Ugalde2013Synthesis17021" class="entry">
	<td>Ferrer-Ugalde, A., Juarez-Perez, E.J., Teixidor, F., Vinas, C. and Nunez, R.</td>
	<td>Synthesis, Characterization, and Thermal Behavior of Carboranyl-Styrene Decorated Octasilsesquioxanes: Influence of the Carborane Clusters on Photoluminescence</td>
	<td>2013</td>
	<td>Chem. Eur. J.<br/>Vol. 19, pp. 17021-17030&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/chem.201302493" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/chem.201302493" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/chem.201302493" > </span></td>
</tr>
<tr id="Kim2013Mechanism2242" class="entry">
	<td>Kim, H.-S., Mora-Sero, I., Gonzalez-Pedro, V., Fabregat-Santiago, F., Juarez-Perez, E.J., Park, N.-G. and Bisquert, J.</td>
	<td>Mechanism of carrier accumulation in perovskite thin-absorber solar cells</td>
	<td>2013</td>
	<td>Nat. Commun.<br/>Vol. 4, pp. 2242&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1038/ncomms3242" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1038/ncomms3242" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1038/ncomms3242" > </span></td>
</tr>
<tr id="Ruisanchez2012Pulses65" class="entry">
	<td>Ruisanchez, E., Arenillas, A., Juarez-Perez, E.J. and Menendez, J.A.</td>
	<td>Pulses of microwave radiation to improve coke grindability</td>
	<td>2012</td>
	<td>Fuel<br/>Vol. 102, pp. 65-71&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1016/j.fuel.2012.07.030" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1016/j.fuel.2012.07.030" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1016/j.fuel.2012.07.030" > </span></td>
</tr>
<tr id="Menendez2012Microwave3555" class="entry">
	<td>Menendez, J.A., Juarez-Perez, E.J., Ruisanchez, E., Calvo, E.G. and Arenillas, A.</td>
	<td>A Microwave based method for the synthesis of carbon xerogel spheres</td>
	<td>2012</td>
	<td>Carbon<br/>Vol. 50, pp. 3555-3560&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1016/j.carbon.2012.03.027" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1016/j.carbon.2012.03.027" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1016/j.carbon.2012.03.027" > </span></td>
</tr>
<tr id="Fernandez2012Electrochemical1067" class="entry">
	<td>Fernandez, P.S., Castro, E.B., Real, S.G., Visintin, A., Arenillas, A., Calvo, E.G., Juarez-Perez, E.J., Menendez, A.J. and Martins, M.E.</td>
	<td>Electrochemical behavior and capacitance properties of carbon xerogel/multiwalled carbon nanotubes composites</td>
	<td>2012</td>
	<td>J. Solid State Electrochem.<br/>Vol. 16(3), pp. 1067-1076&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1007/s10008-011-1487-4" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1007/s10008-011-1487-4" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1007/s10008-011-1487-4" > </span></td>
</tr>
<tr id="Farras2012Metallacarboranes3445" class="entry">
	<td>Farras, P., Juarez-Perez, E.J.,* Lep&#353;ik, M., Luque, R., Nunez, R. and Teixidor, F.</td>
	<td>Metallacarboranes and their interactions: theoretical insights and their applicability</td>
	<td>2012</td>
	<td>Chem. Soc. Rev.<br/>Vol. 41, pp. 3445-3463&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1039/c2cs15338f" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1039/c2cs15338f" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1039/c2cs15338f" > </span></td>
</tr>
<tr id="Juarez-Perez2012Grafting277" class="entry">
	<td>Juarez-Perez, E.J., Granier, M., Vinas, C., Mutin, P.H. and Nunez, R.</td>
	<td>Grafting of Metallacarboranes onto Self-Assembled Monolayers Deposited on Silicon Wafers.</td>
	<td>2012</td>
	<td>Chem. Asian J.<br/>Vol. 7, pp. 277-281&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/asia.201100750" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/asia.201100750" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/asia.201100750" > </span></td>
</tr>
<tr id="Ferrer-Ugalde2012Synthesis544" class="entry">
	<td>Ferrer-Ugalde, A., Juarez-Perez, E.J., Teixidor, F., Vinas, C., Sillanp&auml;&auml;, R., Perez-Inestrosa, E. and Nunez, R.</td>
	<td>Synthesis and characterization of new fluorescent styrene-containing carborane derivatives: the singular quenching role of a phenyl substituent.</td>
	<td>2012</td>
	<td>Chem. Eur. J.<br/>Vol. 18(2), pp. 544-553&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/chem.201101881" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/chem.201101881" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/chem.201101881" > </span></td>
</tr>
<tr id="Juarez-Perez2011Unique11497" class="entry">
	<td>Juarez-Perez, E.J., Aragoni, M.C., Arca, M., Blake, A.J., Devillanova, F.A., Garau, A., Isaia, F., Lippolis, V., Nunez, R., Pintus, A. and Wilson, C.</td>
	<td>A Unique Case of Oxidative Addition of Interhalogens IX (X=Cl, Br) to Organodiselone Ligands: Nature of the Chemical Bonding in Asymmetric I-Se-X Polarised Hypervalent Systems</td>
	<td>2011</td>
	<td>Chem. Eur. J.<br/>Vol. 17(41), pp. 11497-11514&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/chem.201100970" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/chem.201100970" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/chem.201100970" > </span></td>
</tr>
<tr id="Calvo2011Fast541" class="entry">
	<td>Calvo, E.G., Juarez-Perez, E.J., Menendez, J.A. and Arenillas, A.</td>
	<td>Fast microwave-assisted synthesis of tailored mesoporous carbon xerogels.</td>
	<td>2011</td>
	<td>J. Colloid Interface Sci.<br/>Vol. 357(2), pp. 541-547&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1016/j.jcis.2011.02.034" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1016/j.jcis.2011.02.034" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1016/j.jcis.2011.02.034" > </span></td>
</tr>
<tr id="Lufrano2011Carbon596" class="entry">
	<td>Lufrano, F., Staiti, P., Calvo, E.G., Juarez-Perez, E.J., Menendez, J.A. and Arenillas, A.</td>
	<td>Carbon Xerogel and Manganese Oxide Capacitive Materials for Advanced Supercapacitors</td>
	<td>2011</td>
	<td>Int. J. Electrochem. Sci.<br/>Vol. 6, pp. 596-612&nbsp;</td>
	<td>article <a href="http://www.electrochemsci.org/papers/vol6/6030596.pdf">URL</a>&nbsp;</td>
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-uri="http://www.electrochemsci.org/papers/vol6/6030596.pdf" ></div> </td>
</tr>
<tr id="Menendez2011Ball346" class="entry">
	<td>Menendez, J.A., Juarez-Perez, E.J., Ruisanchez, E., Bermudez, J.M. and Arenillas, A.</td>
	<td>Ball lightning plasma and plasma arc formation during the microwave heating of carbons</td>
	<td>2011</td>
	<td>Carbon<br/>Vol. 49(1), pp. 346-349&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1016/j.carbon.2010.09.010" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1016/j.carbon.2010.09.010" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1016/j.carbon.2010.09.010" > </span></td>
</tr>
<tr id="Nunez2010Decorating9993" class="entry">
	<td>Nunez, R., Juarez-Perez, E.J., Teixidor, F., Santillan, R., Farfan, N., Abreu, A., Yepez, R. and Vinas, C.</td>
	<td>Decorating poly(alkyl aryl-ether) dendrimers with metallacarboranes.</td>
	<td>2010</td>
	<td>Inorg. Chem.<br/>Vol. 49(21), pp. 9993-10000&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/ic101306w" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/ic101306w" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/ic101306w" > </span></td>
</tr>
<tr id="Juarez-Perez2010Precise3305" class="entry">
	<td>Juarez-Perez, E.J., Calvo, E.G., Arenillas, A. and Menendez, J.A.</td>
	<td>Precise determination of the point of sol-gel transition in carbon gel synthesis using a microwave heating method</td>
	<td>2010</td>
	<td>Carbon<br/>Vol. 48, pp. 3305-3308&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1016/j.carbon.2010.05.013" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1016/j.carbon.2010.05.013" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1016/j.carbon.2010.05.013" > </span></td>
</tr>


<tr id="Fernandez2010Procesos2" class="entry">
	<td>Fern&aacute;ndez, Y., Fidalgo, B., Zubizarreta, L., Berm&uacute;dez, J., Calvo, E., Ruis&aacute;nchez, E., Ju&aacute;rez-P&eacute;rez, E., Arenillas, A. and Men&eacute;ndez, J.</td>
	<td>Procesos t&eacute;rmicos asistidos por microondas sobre materiales carbonosos</td>
	<td>2010</td>
	<td>Boletín GEC (16), pp. 2-7&nbsp;</td>
	<td>article <a href="http://www.gecarbon.org/Boletines/articulos/boletinGEC_016_art.1.pdf">URL</a>&nbsp;</td>
	<td></td>
</tr>



<tr id="Juarez-Perez2010Role2385" class="entry">
	<td>Juarez-Perez, E.J., Nunez, R., Vinas, C., Sillanp&auml;&auml;, R. and Teixidor, F.</td>
	<td>The Role of C-H...H-B Interactions in Establishing Rotamer Configurations in Metallabis(dicarbollide) Systems</td>
	<td>2010</td>
	<td>Eur. J. Inorg. Chem.<br/>Vol. -(16), pp. 2385-2392&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/ejic.201000157" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/ejic.201000157" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/ejic.201000157" > </span></td>
</tr>
<tr id="Juarez-Perez2010Anchoring12185" class="entry">
	<td>Juarez-Perez, E.J., Mutin, P.H., Granier, M., Teixidor, F. and Nunez, R.</td>
	<td>Anchoring of Phosphorus-Containing Cobaltabisdicarbollide Derivatives to Titania Surfaces</td>
	<td>2010</td>
	<td>Langmuir<br/>Vol. 26(14), pp. 12185-12189&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/la100724r" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/la100724r" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/la100724r" > </span></td>
</tr>
<tr id="Juarez-Perez2010Polyanionic150" class="entry">
	<td>Juarez-Perez, E.J., Vinas, C., Teixidor, F., Santillan, R., Farfan, N., Abreu, A., Yepez, R. and Nunez, R.</td>
	<td>Polyanionic Aryl Ether Metallodendrimers Based on Cobaltabisdicarbollide Derivatives. Photoluminescent Properties</td>
	<td>2010</td>
	<td>Macromolecules<br/>Vol. 43(1), pp. 150-159&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/ma9019575" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/ma9019575" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/ma9019575" > </span></td>
</tr>
<tr id="Juarez-Perez2009Metalodendrimeros" class="entry">
	<td>Juarez-Perez, E.J.</td>
	<td>Metalodendrimeros y Materiales Nanoestructurados que Incorporan Clusteres de Boro.</td>
	<td>2009</td>
	<td><i>School</i>: ICMAB-CSIC&nbsp;</td>
	<td>phdthesis <a href="http://hdl.handle.net/10803/3294">URL</a>&nbsp;</td>
<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-uri="http://hdl.handle.net/10803/3294" ></div> </td>
</tr>
<tr id="Juarez-Perez2009First1764" class="entry">
	<td>Juarez-Perez, E.J., Vinas, C., Teixidor, F. and Nunez, R.</td>
	<td>First example of the formation of a Si-C bond from an intramolecular Si-H...H-C diyhydrogen interaction in a metallacarborane: A theoretical study</td>
	<td>2009</td>
	<td>J. Organomet. Chem.<br/>Vol. 694(11), pp. 1764-1770&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1016/j.jorganchem.2008.12.022" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1016/j.jorganchem.2008.12.022" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1016/j.jorganchem.2008.12.022" > </span></td>
</tr>
<tr id="Juarez-Perez2009Polyanionic5550" class="entry">
	<td>Juarez-Perez, E.J., Vinas, C., Teixidor, F. and Nunez, R.</td>
	<td>Polyanionic Carbosilane and Carbosiloxane Metallodendrimers Based on Cobaltabisdicarbollide Derivatives</td>
	<td>2009</td>
	<td>Organometallics<br/>Vol. 28(18), pp. 5550-5559&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/om9005643" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/om9005643" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/om9005643" > </span></td>
</tr>
<tr id="Gonzalez-Campo2008Carboranyl8458" class="entry">
	<td>Gonzalez-Campo, A., Juarez-Perez, E.J., Vinas, C., Boury, B., Sillanpaa, R., Kivekas, R. and Nunez, R.</td>
	<td>Carboranyl Substituted Siloxanes and Octasilsesquioxanes: Synthesis, Characterization, and Reactivity</td>
	<td>2008</td>
	<td>Macromolecules<br/>Vol. 41(22), pp. 8458-8466&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1021/ma801483c" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1021/ma801483c" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1021/ma801483c" > </span></td>
</tr>
<tr id="Juarez-Perez2008Controlled4924" class="entry">
	<td>Juarez-Perez, E.J., Vinas, C., Gonzalez-Campo, A., Teixidor, F., Sillanp&auml;&auml;, R., Kivek&auml;s, R. and Nunez, R.</td>
	<td>Controlled direct synthesis of C-mono- and C-disubstituted derivatives of [3,3'-Co(1,2-C<sub>2</sub>B<sub>9</sub>H<sub>11</sub>)<sub>2</sub>]<sup>-</sup> with organosilane groups: theoretical calculations compared with experimental results.</td>
	<td>2008</td>
	<td>Chem. Eur. J.<br/>Vol. 14(16), pp. 4924-4938&nbsp;</td>
	<td>article <a href="http://dx.doi.org/10.1002/chem.200702013" >DOI</a> &nbsp;</td> 
 	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.1002/chem.200702013" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.1002/chem.200702013" > </span></td>
</tr>

<tr id="Juarez-Perez2006Nuevos" class="entry">
	<td>Juarez-Perez, E.J.</td>
	<td>Nuevos C-derivados de cobaltocarborano con grupos silano enfocados a la funcionalizaci&oacute;n de estructuras dendrim&eacute;ricas</td>
	<td>2006</td>
	<td><i>School</i>: ICMAB-CSIC;</td>
	<td>mastersthesis <a href="http://dx.doi.org/10.5281/zenodo.4612099" >DOI</a></td>
	<td> <div class='altmetric-embed' data-badge-type='donut' data-hide-no-mentions='true' data-badge-popover="left" data-doi="10.5281/zenodo.4612099" ></div> <span class="__dimensions_badge_embed__" data-style="small_rectangle" data-doi="10.5281/zenodo.4612099" > </span></td>
</tr>


</tbody>
</table>

<footer>
 <small>Created by <a href="http://jabref.sourceforge.net">JabRef</a> on 27/12/2021.</small>
</footer>

<!-- file generated by JabRef -->

</body>
</html>

