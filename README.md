<!DOCTYPE html>
<html lang="hu">
<head>
<meta charset="UTF-8" name="viewport" content="width=device-width, initial-scale=1.0">
<style>
* {
  box-sizing: border-box;
}

#tag {
line-height: 20px; /*must be given in px, used in getPos(e) function */
}

#hedit {
user-select:text;  
line-height: 15px; /*must be given in px, used in getPos(e) function */
}

#dom {
line-height: 15px; /*must be equal to hedit*/
}

.row::after {
  content: "";
  clear: both;
  display: table;
}

[class*="col-"] {
  float: left;
  padding: 15px;
}

html {
  font-family: "Lucida Sans", sans-serif;
}

.header {
  background-color: #33cccc;
  color: #ffffff;
  padding: 15px;
}

.menu ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.menu li {
  padding: 8px;
  margin-bottom: 7px;
  background-color: #33b5e5;
  color: #ffffff;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
  /*white-space: normal;*/
  text-align: center;
  /*overflow: hidden;
  white-space: nowrap;
  text-overfow: ellipsis;*/
}

.menu li:hover {
  background-color: #0099cc;
  
}

.menu ul ul {
   display: none;
}

.menu li:hover > ul {    
	display: block;
	margin-left: 5px;
	margin-right: 40px;
}



.aside {
  background-color: #33b5e5;
  padding: 15px;
  color: #ffffff;
  text-align: center;
  font-size: 14px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}

.footer {
  background-color: #33cccc;
  color: #ffffff;
  text-align: center;
  font-size: 12px;
  padding: 15px;
}

/* For mobile phones: */
[class*="col-"] {
  width: 100%;
}

@media only screen and (min-width: 600px) {
  /* For tablets: */
  .col-s-1 {width: 8.33%;}
  .col-s-2 {width: 16.66%;}
  .col-s-3 {width: 25%;}
  .col-s-4 {width: 33.33%;}
  .col-s-5 {width: 41.66%;}
  .col-s-6 {width: 50%;}
  .col-s-7 {width: 58.33%;}
  .col-s-8 {width: 66.66%;}
  .col-s-9 {width: 75%;}
  .col-s-10 {width: 83.33%;}
  .col-s-11 {width: 91.66%;}
  .col-s-12 {width: 100%;}
}
@media only screen and (min-width: 768px) {
  /* For desktop: */
  .col-1 {width: 8.33%;}
  .col-2 {width: 16.66%;}
  .col-3 {width: 25%;}
  .col-4 {width: 33.33%;}
  .col-5 {width: 41.66%;}
  .col-6 {width: 50%;}
  .col-7 {width: 58.33%;}
  .col-8 {width: 66.66%;}
  .col-9 {width: 75%;}
  .col-10 {width: 83.33%;}
  .col-11 {width: 91.66%;}
  .col-12 {width: 100%;}
}
</style>
<script language="javascript">
let heditFocus = 0;
let mouseDown = 0;
/*
function markTag(e){
if heditMarked == 0  { 
var x=e.clientX*1;
var y=parseFloat(e.clientY*1);
var cursor="Your Mouse Position Is : " + x + " and " + y;
}
}
*/

function mUp(e){



mouseDown=1;
mouseDown=0;
heditFocus = 1;	
document.getElementById("hedit").focus();
focusHedit(e);


document.getElementById("hedit").focus();
document.getElementById("hedit").focus();
var edlines = document.getElementById("hedit").value.split("\n");
var editorCords = document.getElementById("hedit").getClientRects()[0];
var textarea = document.getElementById ("hedit");
var style = window.getComputedStyle (textarea, null);
var rh = style.getPropertyValue ("line-height"); // get the line height, if given in px
rh=rh.replace("px", "");
lih=parseFloat(rh);

//var rh = textarea.style.lineHeight;
var x=e.clientX*1;
var ex=parseFloat(editorCords.x*1);
var y=parseFloat(e.clientY*1);
var ey=parseFloat(editorCords.y*1);
var rp=Math.floor((y - ey)/lih); //number of the line where the cursor is

/*
// re-use canvas object for better performance
var font = document.getElementById("hedit").style.font;
//let canvas = getTextWidth.canvas;
//document.getElementById("tag").value="mukodik";
let canvas = document.createElement("canvas");
document.getElementById("hedit").appendChild(canvas);
  const context = canvas.getContext("2d");
  context.font = font;
  const metrics = context.measureText(line);
var width = metrics.width;
var curPos = Math.floor(width/(x-ex)*line.length);  //this is correct just if the typeface is monospace(letter-width is equal)
*/
let linebrake=0;
let startp=0;
for(var edx = 0; edx < rp; edx++) {  
        if (edx+linebrake >= rp){break;} 
        startp += (edlines[edx].length+1);
        if (edlines[edx].length>document.getElementById("hedit").cols){
          linebrake += Math.floor(edlines[edx].length/document.getElementById("hedit").cols);
          
          }       
        
    }

//var curPos= textarea.selectionEnd-startp;

var edline = edlines[rp-linebrake];
var curPos= textarea.selectionStart-startp;



//var curpos= 10;
//if (line == "/n"){line = "nemures";}
//console.log(edlines);
const Aline = edline.split(""); //Array.from(edline);



var typ="";
var t="";
var t2="";

//document.getElementById("tag").value="mukodik";  //"mukodik\n   \n \n ";
//document.getElementById("hedit").focus();

var ct="";  //collecting the text

for(let i = curPos-1; i >= 0; i--) {

        if(i < 0) {
            break;
        }
		
        if (Aline[i]== " "){
		  t2=ct;
		  ct="";
		} else if (Aline[i]== "<"){
		  t=ct;
		  ct="";
		  if(t2==""){typ="tag";break;} else {typ="par";break;}
		} else if (Aline[i]=='"'){
		  t2=ct;
		  ct="";
		  typ="pav"; break;
		} else if (Aline[i]=="="){
		  if(t2==""){t2=ct;ct="";typ="paf";break;} else {t=ct;ct="";break;}
		} else if (Aline[i]==">"){
		  t=ct;
		  ct="";
		  typ="dat"; break;
		} else if (Aline[i]=="/"){
		  t=ct;
		  ct="";
		  typ="ctg"; break;		
		} else {
			ct=Aline[i]+ct;
			}
    }
	



 
var att = [ 
['p', 'class', 'id', 'style="text-align: center/left/right; font-size:12px; color:red; background-color: yellow;"', 'tabindex', 'title', 'accesskey', 'contenteditable', 'dir', 'contextmenu', 'draggable', 'dropzone', 'hidden', 'translate', 'onblur', 'onchange', 'onfocus', 'onload', 'onsearch', 'onselect', 'onsubmit', 'onkeypress', 'onclick', 'ondblclick', 'ondrag', 'ondragenter', 'ondragleave', 'ondragover', 'onmouseover', 'onwheel', 'onscroll', 'oncopy', 'onpaste', 'onplay', 'onabort', 'onpause', 'onvolumechange', 'onerror', 'onshow', 'ontoggle']

,['font', 'face', 'size', 'color'] //<font>

,['h1', 'class', 'id', 'style="text-align: center/left/right; font-size:12px; color:red; background-color: yellow;"', 'tabindex', 'title', 'accesskey', 'contenteditable', 'dir', 'contextmenu', 'draggable', 'dropzone', 'hidden', 'translate', 'onblur', 'onchange', 'onfocus', 'onload', 'onsearch', 'onselect', 'onsubmit', 'onkeypress', 'onclick', 'ondblclick', 'ondrag', 'ondragenter', 'ondragleave', 'ongragover', 'onmouseover', 'onwheel', 'onscroll', 'oncopy', 'onpaste', 'onplay', 'onabort', 'onpause', 'onvolumechange', 'onerror', 'onshow', 'ontoggle']
,['a', 'download', 'href', 'hreflang', 'media', 'ping', 'rel', 'target', 'type',        'class', 'id', 'style', 'tabindex', 'title', 'accesskey', 'contenteditable', 'dir', 'contextmenu', 'draggable', 'dropzone', 'hidden', 'translate', 'onblur', 'onchange', 'onfocus', 'onload', 'onsearch', 'onselect', 'onsubmit', 'onkeypress', 'onclick', 'ondblclick', 'ondrag', 'ondragenter', 'ondragleave', 'ongragover', 'onmouseover', 'onwheel', 'onscroll', 'oncopy', 'onpaste', 'onplay', 'onabort', 'onpause', 'onvolumechange', 'onerror', 'onshow', 'ontoggle']
,['b', 'class', 'id', 'style', 'tabindex', 'title', 'accesskey', 'onmouseover']
,['i', 'class', 'id', 'style', 'tabindex', 'title', 'accesskey', 'onmouseover']
,['u', 'class', 'id', 'style', 'tabindex', 'title', 'accesskey', 'onmouseover']
,['br', 'clear=all/left/right/none'] //<br>


,['hr', 'class', 'id', 'style', 'tabindex', 'title', 'accesskey', 'onmouseover']
,['wbr', 'class', 'id', 'style', 'tabindex', 'title', 'accesskey', 'onmouseover']
,['span', 'class', 'id', 'style="text-align: center/left/right; font-size:12px; color:red; background-color: yellow;"', 'tabindex', 'title', 'accesskey', 'onmouseover'] //<span>

,['div', 'class', 'id', 'style="float:left/right; text-align:center/left/right; font-size:12px; color:red; background-color: yellow;"', 'tabindex', 'title', 'accesskey', 'onmouseover']//<div>

,['nav', 'class', 'id', 'style', 'tabindex', 'title', 'accesskey', 'onmouseover']//nav search-engines-friendly
,['main', 'class', 'id', 'style', 'tabindex', 'title', 'accesskey', 'onmouseover']//main - the unique content, for search-engines, and against skip-nav functions
,['article', 'class', 'id', 'style', 'tabindex', 'title', 'accesskey', 'onmouseover']
,['object', 'type/data', 'class', 'id', 'style', 'tabindex', 'title', 'accesskey', 'onmouseover']//object
,['ol', 'reversed', 'start="27"', 'type="1/A/a/I/i"', 'class', 'id', 'style="list-style-type:square/disc/circe; line-height:20px; list-style-position:inside/outside;"', 'tabindex', 'title', 'accesskey', 'onmouseover'] //<ol>
,['ul', 'class', 'id', 'style="list-style-type:square/disc/circe; line-height:20px;  list-style-position:inside/outside;"', 'tabindex', 'title', 'accesskey', 'onmouseover'] //<ul>
,['li', 'value="5"', 'class', 'id', 'style="list-style-image: url("im.png"); line-height:20px;"', 'tabindex', 'title', 'accesskey', 'onmouseover'] //<li>
]; //https://www.w3docs.com/learn-html/html-map-tag.html




/*
par    				parameter   <..?1.. ..?2..
pav, vagy paf		paramvalue  ?1=..?2..	paf, ha függvény, és nincs előtte
tag					tag         <..?..	gyakori tag-ek felsorolása, ha t<>"" akkor a kezdetükre szűrje
dat					data		?>...    	nincs teendő
ctg					ctag		<\..?..		
ctg								\			melyik a nyitója - keresni , ha utána még nincsen semmit="")
		
*/
var tt="";
var test="";
//var tti="";	
	if (typ== "tag"){
      if (t==""||t=="b"||t=="h"||t=="hr"){
        tt="<p></>\n<font></>\n<h1></>\n<a></>\n<b></>\n<i></>\n<u></>\n<br></>\n<hr></>\n<wbr></>\n<span></>\n<div></>\n<nav></>\n<main></>\n<article></>\n<object></>\n<ol></>\n<ul></>\n<li></>\n<table></>\n<tr></>\n<th></>\n<td></>\n<pre></>\n<body></>\n<img></>\n<label></>\n<form></>\n<input></>\n<fieldset></>\n<legend></>\n<select></>\n<option></>\n<optgroup></>\n<textarea></>\n<time datetime></\n";
      }else{
      for (var i=0; i<att.length; i++){
        test=att[i][0].slice(0, t.length);
        if (att[i][0].slice(0, t.length)==t){          
          tt=tt+"<"+att[i][0]+"></"+att[i][0]+">\n"
          }
        } //for
      }//if tt else      
      //tt=tti+tt;
/*
	} else if (typ== "<"){
		  t=ct;
		  ct="";
		  if(t2==""){typ="tag";}else{typ="par";}
	} else if (typ=='"'){
		  t2=ct;
		  ct="";
		  typ="pav";
	} else if (typ=="="){
		  if(t2==""){t2=ct;ct="";typ="paf";}else{t=ct;ct="";}
	} else if (typ==">"){
		  t=ct;
		  ct="";
		  typ="dat";
*/
	} else if (typ=="ctg"){		  
     if (t==""||t=="b"||t=="h"||t=="hr"){
        tt="<p></>\n<font></>\n<h1></>\n<a></>\n<b></>\n<i></>\n<u></>\n<br></>\n<hr></>\n<wbr></>\n<span></>\n<div></>\n<nav></>\n<main></>\n<article></>\n<object></>\n<ol></>\n<ul></>\n<li></>\n<table></>\n<tr></>\n<th></>\n<td></>\n<pre></>\n<body></>\n<img></>\n<label></>\n<form></>\n<input></>\n<fieldset></>\n<legend></>\n<select></>\n<option></>\n<optgroup></>\n<textarea></>\n<time datetime></\n";
      }else{
      for (var i=0; i<att.length; i++){
        test=att[i][0].slice(0, t.length);
        if (att[i][0].slice(0, t.length)==t){          
          tt=tt+"/"+att[i][0]+">\n"
          }
        } //for
      }//if tt else      


//      var maxj=0;
//      for (var i=rp; i>=0; i--){
//        maxj=Aline[i].length-1;
//        for (var j=maxj; j>=0; j--){
//          test=Aline[i][j];
//          if (Aline[i][j]="<"){          
//            if (Aline[i][min(maxj,j+1)]==t.indexOf(0) && Aline[i][min(maxj,j+2)]==t.indexOf(1) && Aline[i][min(maxj,j+3)]==t.indexOf(2) && Aline[i][min(maxj,j+4)]==t.indexOf(3)){
              
//              }
//            } //if
//          } //for j
//        } //for i
	} else {
    tt="<p></>\n<font></>\n<h1></>\n<a></>\n<b></>\n<i></>\n<u></>\n<br></>\n<hr></>\n<wbr></>\n<span></>\n<div></>\n<nav></>\n<main></>\n<article></>\n<object></>\n<ol></>\n<ul></>\n<li></>\n<table></>\n<tr></>\n<th></>\n<td></>\n<pre></>\n<body></>\n<img></>\n<label></>\n<form></>\n<input></>\n<fieldset></>\n<legend></>\n<select></>\n<option></>\n<optgroup></>\n<textarea></>\n<time datetime></\n";
	}	
	
/*	
var att = ['p', 'font', 'h1', 'a', 'b', 'i', 'u', 'br', 'hr', 'wbr', 'span', 'div', 'nav', 'main', 'article', 'object', 'ol', 'ul', 'li', 'table', 'tr', 'th', 'td', 'pre', 'body', 'img', 'label', 'form', 'input', 'fieldset', 'legend', 'select', 'option', 'optgroup', 'textarea', 'time datetime="2018-09-28 19:00"'];
*/

//document.getElementById("tag").focus();
//tt="mukodik\n   \n \n ";		
document.getElementById("tag").value=tt;
document.getElementById("hedit").focus();
//document.getElementById("tag").focus();

} //funct mUp
 


//function mUp(e){
//mouseDown=0;
//focusHedit(e);
//}

function focusHedit(e){
//alert(cursor);
if (e.ctrlKey || e.shiftKey || mouseDown==1) {
heditFocus = 1;
//document.getElementById("hedit").focus();
} else{
//heditFocus = 0;
//document.getElementById("tag").focus();
}
  
}//focusHedit(e){

function hedKdown(e){
if (e.keyCode == 9 || e.which == 9) { // Ha a Tab billentyűt nyomták le
    e.preventDefault(); // Megakadályozzuk az alapértelmezett viselkedést
    var start = this.selectionStart; // A kijelölés kezdő pozícióját elmentjük
    var end = this.selectionEnd; // A kijelölés végének pozícióját elmentjük
    this.value = this.value.substring(0, start) + "   " + this.value.substring(end); // Beszúrjuk a tabulátor karaktert a kijelölt szöveg helyére
    this.selectionEnd = start + 3; // Visszaállítjuk a kijelölést a beszúrt karakter után
  }
  
} //hedKdown(e)
 
function hedPaste(e){
/*
  var paste = (e.clipboardData || window.clipboardData).getData("text"); // A beillesztett szöveg lekérése
  var edlines = document.getElementById("hedit").value.split("\n");
  document.getElementById("hedit").value = "";
  for(var i = 0; i < edlines.length; i++) {  
    edlines[i].replace(/\t/g, " "); // Lecseréljük a tabulátorokat szóközre)    
    document.getElementById("hedit").value += edlines[i]+"\n";    
    }  

  //paste.replace(/\t/g, " "); // Lecseréljük a tabulátorokat szóközre
  //e.clipboardData.setData("text/plain", paste); // Beállítjuk a vágólap tartalmát
  //e.preventDefault(); // Megakadályozzuk az alapértelmezett viselkedést
*/
} //funct hedPaste



//function focusTag(e){
//if (!e.ctrlKey && !e.shiftKey && e.which<>1 && e.which<>3) {
//heditFocus = 0;
//}
//}
 
function markTag(e){
if (heditFocus == 0)  {

var lines = document.getElementById("tag").value.split("\n");

var editorCords = document.getElementById("tag").getClientRects()[0];

var textarea = document.getElementById ("tag");
var style = window.getComputedStyle (textarea, null);
var rh = style.getPropertyValue ("line-height"); // get the line height, if given in px
rh.replace("px", "");
lih=parseFloat(rh);
//var rh = textarea.style.lineHeight;

var x=e.clientX*1;
var y=parseFloat(e.clientY*1);
var ey=parseFloat(editorCords.y*1);
var rp=Math.floor((y - ey)/lih); //number of the line where the cursor is
if (rp<0){rp=0;}
var cursor="Your Mouse Position Is : " + x + " and " + rp;

//alert (cursor);
var linenum = Math.min(lines.length-1, rp);
var startPos = 0;
for(var x = 0; x < lines.length; x++) {
        if(x == linenum) {
            break;
        }
        startPos += (lines[x].length+1);
    }
document.getElementById("tag").focus();
document.getElementById("tag").selectionStart = startPos;
document.getElementById("tag").selectionEnd = startPos + lines[linenum].length;
//document.getElementById("hedit").focus();
}
}


function getPos(e){

heditFocus = 0;

var lines = document.getElementById("tag").value.split("\n");
//var rh = document.getElementById("tag").style.lineHeight;//lines.length;

var editorCords = document.getElementById("tag").getClientRects()[0];
//var cursorCords = window.getSelection().getRangeAt(0).getClientRects()[0];
//var ch=cursorCords.height*1;
//var line=0;    
//if (editorCords && cursorCords) {
//   line = Math.floor((cursorCords.y - editorCords.y) / cursorCords.height);
//   alert(line);
//}


var textarea = document.getElementById ("tag");
var style = window.getComputedStyle (textarea, null);
var rh = style.getPropertyValue ("line-height"); // get the line height, if given in px
rh.replace("px", "");
lih=parseFloat(rh);
//var rh = textarea.style.lineHeight;

var x=e.clientX*1;
var y=parseFloat(e.clientY*1);
var ey=parseFloat(editorCords.y*1);
var rp=Math.floor((y - ey)/lih);
if (rp<0){rp=0;}
var cursor="Your Mouse Position Is : " + x + " and " + rp;

//alert (cursor);
var linenum = Math.min(lines.length-1, rp);
var startPos = 0;
for(var x = 0; x < lines.length; x++) {
        if(x == linenum) {
            break;
        }
        startPos += (lines[x].length+1);
    }
document.getElementById("tag").focus();
document.getElementById("tag").selectionStart = startPos;
document.getElementById("tag").selectionEnd = startPos + lines[linenum].length;
}

function getli()
{
/*
var x=document.finli.inli.value;
var lines = document.getElementById("hedit").value.split("\n");
var lastLine = lines[lines.length - 1];
//alert(lines[x]);
document.getElementById("tag").value="<p></p>\n<h></h>\n<br>";
*/
}

</script>
</head>
<body>

<div class="header">
  <h1>HTML editor</h1>
</div>

<div class="row">
  <div class="col-3 col-s-3 menu">
    <p><TEXTAREA name="dom" rows="20" cols="30">
    
   html
   |
   |
   
   
   </TEXTAREA></p>
  </div>

  <div class="col-6 col-s-9">    
    <p><TEXTAREA id="hedit" name="hedit" rows="20" cols="75" onpaste=hedPaste(event) onkeydown=focusHedit(event) onkeydown=hedKdown(event) onmouseup=mUp(event) onkeyup=focusHedit(event) onmousemove=markTag(event)>
      <!DOCTYPE html>
      <html lang="hu">
      <head>
      <meta charset="UTF-8" name="viewport" content="width=device-width, initial-scale=1.0">
      <style>

    </style>
    <script>

      </script>
    <head>
    <body>

    </body>
    </html>


   </TEXTAREA>
   <form name = "finli"><INPUT type="button" name="btn" value="Sor" onclick=getli()><input type="text" name = "inli"></form>
   </p>
  </div>

  <div class="col-3 col-s-12">
    <div class="aside">
	    <p><TEXTAREA id="tag" style="font-size: 14px" name="tag" rows="20" cols="30" onmouseover=getPos(event)>
    
  
  
   
       </TEXTAREA></p>  
    </div>
  </div>
</div>

<div class="footer">
  <p>The source and contett of this site may be used free for any goal.</p>
</div>

</body>
</html>
