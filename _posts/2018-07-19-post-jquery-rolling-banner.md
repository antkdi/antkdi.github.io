---
title: "jQuery를 이용한 롤링 배너"
comment: true
date: 2018-07-19
categories: [Programing, Javascript]
tags: [JQuery, Rolling Banner]

---

## jQuery를 이용한 롤링 배너

``` javascript 
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
 <head>
  <title> New Document </title>
  <meta name="Generator" content="EditPlus">
  <meta name="Author" content="">
  <meta name="Keywords" content="">
  <meta name="Description" content="">
  <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.4/jquery.min.js"></script>
  <script>
    var thisimg = 1;
 var maximg = 4;
 var boxfocus = false;
 $(document).ready(function() {
  setInterval(imgrotation, 3000);
  for(var i=0;i < 4;i++) {
   $("#img0"+(i+1)).bind('mouseover', function() {
    boxfocus = true;
    thisidx = $(this).attr('id').replace(/img0/,'');
    if(thisimg != thisidx) {
     $("#img0"+thisimg).attr("src","../img/"+thisimg+"m.png");
     $("#img0"+thisidx).hide();
     $("#img0"+thisidx).attr("src","../img/"+thisidx+".png");
     $("#img0"+thisidx).fadeIn('slow');
     thisimg = parseInt(thisidx);
    } 
   }).bind('mouseout', function() {
    boxfocus = false;
   });
  }
 });
 function getnextimg(ix) {
  if(ix + 1 > maximg) return 1;
  return ix+1;
 }
 function imgrotation() {
  if(!boxfocus) {
   $("#img0"+thisimg).attr("src","../img/"+thisimg+"m.png");
   nextimg = getnextimg(thisimg);
   $("#img0"+nextimg).hide();
   $("#img0"+nextimg).attr("src","../img/"+nextimg+".png");
   $("#img0"+nextimg).fadeIn('slow');
   thisimg = nextimg;
  }
 }
  </script>
 <style>
p.nowrap{ 
white-space: nowrap; 
margin:0; 
padding:0; 
} 
 </style>
 </head>
 <body>
<div id="adbox">
 <p class="nowrap"><img id='img01' src="../img/1.png" /><img id='img02' src="../img/2m.png" /><img id='img03' src="../img/3m.png" /><img id='img04' src="../img/4m.png" /></p>
</div>
 </body>
</html>

```
