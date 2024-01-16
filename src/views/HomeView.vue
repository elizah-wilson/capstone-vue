<script setup>
import $ from 'jquery'
import { ref, onMounted } from 'vue'
import Header from '../components/Header.vue'


onMounted(() => {
  (function( $ ) {
  
  // console.clear()
  // console.log('svgColor')
  
  var mainHolder, colorHolder
  var btnRandom, btnClear
  var svgObject, svgOutline, svgColor 
  var swatchUp, swatchDown
  var fillSpeed = 0.15
  var chosenColor = '#FFFFFF'
  var colors = ['#FFFFFF', '#8E53A1', '#6ABD46', '#71CCDC', '#F7ED45', '#F7DAAF', '#EC2527', '#F16824', '#CECCCC', '#5A499E', '#06753D', '#024259', '#FDD209', '#7D4829', '#931B1E', '#B44426', '#979797', '#C296C5', '#54B948', '#3C75BB', '#F7ED45', '#E89D5E', '#F26F68', '#F37123', '#676868', '#9060A8', '#169E49', '#3CBEB7', '#FFCD37', '#E5B07D', '#EF3C46', '#FDBE17', '#4E4D4E', '#6B449B', '#BACD3F', '#1890CA', '#FCD55A', '#D8C077', '#A62E32', '#F16A2D', '#343433', '#583E98', '#BA539F', '#9D2482', '#DD64A5', '#DB778D', '#EC4394', '#E0398C', '#68AF46', '#4455A4', '#FBEE34', '#AD732A', '#D91E36', '#F99B2A']
  var closeOffset

  function swatchClick(){
    chosenColor = $(this).data('color')
    // console.log(chosenColor)
    TweenMax.to(colorHolder, fillSpeed, { backgroundColor:chosenColor })
  }
  function swatchMove(e){
    var moveTo = (e.type == 'mouseenter')? swatchUp: swatchDown;
    TweenMax.to('.swatchHolder', 0.5, moveTo);
  }
  
  function colorMe() {
    TweenMax.to(this, fillSpeed, { fill: chosenColor });
  }
  function colorRollover(e){
    var rollover = (e.type == 'mouseenter')? {scale:1.2}:{scale:1};
    TweenMax.to($(this), 0.05, rollover); 
  }
  
  // function download(){
  //   var svgInfo = document.querySelector('svg').outerHTML
  //   var parser = new DOMParser();
  //   var xmlDoc = parser.parseFromString(svgInfo,"text/xml");
  //   var dl = document.createElement("a");
  //   document.body.appendChild(dl); // This line makes it work in Firefox.
  //   dl.setAttribute("href", "data:image/svg+xml;base64,"+xmlDoc);
  //   dl.setAttribute("download", "test.svg");
  //   dl.click();
  //}

  /*  }
  function svgDownloadSVG() {
   var svgInfo = $(svgObject).clone();
   console.clear()
   console.log(svgInfo)
   $(this).attr({
            href:"data:image/svg+xml;base64,"+svgInfo.toString(),
            download:'coloringBook.svg',
            target:"_blank"
    });
  }   */


  function svgRandom() {
    $(svgColor).each(function(){
      var randomNum = Math.floor((Math.random() * colors.length) + 1);
      TweenMax.to(this, fillSpeed, { fill: colors[randomNum] });
    })
  }

  function svgClear() {
    $(svgColor).each(function(){
      TweenMax.to(this, fillSpeed, { fill: "#FFF" });
    })
  }

  function svgDownloadSVG() {
   var svgInfo = $(svgObject).clone();
   console.clear()
   console.log(svgInfo)
   $(this).attr({
            href:"data:image/svg+xml;base64,"+svgInfo.toString(),
            download:'coloringBook.svg',
            target:"_blank"
    });
  }
 
  $.fn.makeSwatches = function() {
    var swatchHolder = $('<ol/>', {'class': 'swatchHolder'}).appendTo(this)
        colorHolder  = $('<li/>', {'class': 'colorHolder', 'text':'Current Color'}).css('background-color', chosenColor).appendTo(swatchHolder)

        swatchHolder.addClass('swatchHolder')
        colorHolder.addClass('cardHolder')


    $.each(colors, function() {
      var swatch = $('<li/>').appendTo(swatchHolder)
      $(swatch).css('background-color', this)
      $(swatch).data('color', this)
      $(swatch).on('click', swatchClick)
      $(swatch).on('mouseenter mouseleave', colorRollover)
      $(swatch).addClass('color')
    })

    var swatchPos = $('.colorHolder').position()
    var swatchHeight = $('.colorHolder').outerHeight(true) + swatchPos.top
    closeOffset = swatchHeight - $('.swatchHolder').outerHeight()

    $('.swatchHolder').on('mouseenter mouseleave', swatchMove)
    $('.swatchHolder').css('bottom',closeOffset)
    swatchUp   = {css:{bottom:0}}
    swatchDown = {css:{bottom:closeOffset}}
  } 

  $.fn.makeSVGcolor = function(svgURL) {
    mainHolder = this
    // $ - define/access jQuery
    // (selector) to "query" HTML elements
    // action() to be performed on elements
    // load method loads data from a server and puts the returned data into the mainHolder element
    // syntax $(selector).load(URL, data, callback)
    // 
    $( this ).load(svgURL, function() {
      // svgObject is a variable that holdssvg element 
      svgObject  = $('svg', this)
      console.log('svgObject')
      console.log(svgObject)

      svgColor   = $('g#Color', svgObject).children()

      console.log('svgObject.children')
      console.log(svgObject.children())

      console.log('svgColor')
      console.log(svgColor)
      svgOutline = $('g:nth-child(1)', svgObject).children()
      console.log('svgOutline')
      console.log(svgOutline)
      $(svgColor).on('click', colorMe)
      $(mainHolder).makeSwatches()
      $('.swatchHolder')
    });
  }

  $.fn.btnRandom    = function() {
    btnRandom = this
    $(btnRandom).on('click', svgRandom)
  }
  $.fn.btnClear     = function() {
    btnClear = this
    $(btnClear).on('click', svgClear)
  }
  // $.fn.btnDownload  = function(type) {
  //   if(type == 'PNG'){
  //     btnDownloadPNG = this
  //     $(this).on('mouseenter', svgDownloadPNG)
  //   } else {
  //     btnDownloadSVG = this
  //     $(this).on('mouseenter', svgDownloadSVG)
  //   }
  // }

  // $.fn.btnDownload2 = function() {
  //   btnClear = this
  //   $(btnClear).on('click', download)
  // }

$('#ActivityDIV').makeSVGcolor('https://mysvgfiles.s3.us-east-2.amazonaws.com/gardening.svg')
$('#btnRandom').btnRandom()
$('#btnClear').btnClear()
// $('#btnDownloadSVG').btnDownload()
// $('#btnDownloadSVG2').btnDownload2()
}( jQuery ));
})

</script>


<template>
<Header id="header"></Header>
<div class='holder'>
  <div class='Title'>PAGE OF THE DAY</div>
  <div class='bttns-container' id='ActivityDIV'></div>
  <div class='bttns-container'>
    <a id="btnRandom"       class="button">Random Color</a>
    <a id="btnClear"        class="button">Clear Color</a>
    <a id="btnDownloadSVG"  class="button gray">Download SVG</a>
    <a id="btnDownloadSVG2" class="button gray">Download SVG2</a>
  </div>
  <!-- <div id="prompt-container">
      <p id="prompt">Prompt of the Day</p>
      <textarea id="prompt-response" placeholder="Write your musings here..."></textarea>
  </div> -->
</div>
</template>


<style>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
}
/* 
#prompt-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: white;
}

#prompt {
 
}

#prompt-response {
  background-color: #134611;
  cursor:text;
  color: inherit;
  padding: 10px;
} */

.holder{
  width:900px;
  height: 1100px;
  background-color: #0B5D1E;
  /* border-radius: 35px; */
  align-self: center;
  justify-self: center;
  /* -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box; */
  box-sizing: border-box;
  border: 0px solid ;
    
  display: inline-block;
 
  padding: 2vmin;
  
}

.Title{
  
  font-family:'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  font-size: 25px;
  text-align: center;
  margin-bottom: 10px;
  Color: white; 
  
}
 
.bttns-container{ 
  position:relative;
  overflow:hidden;
  border-radius: 20px;
  align-self: center;
  justify-self: center;
   -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  border: 0px;
  margin: auto;
  padding: 2vmin;
 
}

.buttons{
  display:inline-block;
  padding:15px;
  
}

.button{
  transition-duration: 0.4s;
  width:150px;
  padding:10px; 
  background: #7ad0d3; 
  font: 15px Arial, sans-serif;
  height: 40px;
  line-height: 42px;
  border: none;  
    
  outline:none;
 
  text-align: center;
 
  border-radius:40px;
  background: #fff;
  border: px solid #b33a6d;
  color:#b33a6d;
  letter-spacing:1px;
  text-shadow:0;
  
  cursor: pointer;
  transition: all 0.25s ease;
}
  
  
 .button:hover { 
  color: white;
    background: #b33a6d;
} 

.btnClear{
  font: 15px Arial, sans-serif;
}

#ActivityDIV{
  padding:65px; 
  background: white;
  border: 0px;
  height: 690px;
  border-radius: inherit;
  
}

.swatchHolder{
  position:absolute;
  bottom:0px;
  margin:auto;
  left:0px;
  right:0px;
  list-style-type:none;
  text-align:center;
  letter-spacing:1px;
  font-family:Arial;
  display:inline-block;
  padding: 15px;
  width:260px;
  border-radius: 35px 35px 0px 0px;
  color: #232323;
  background-color: white;
  /* background-image: url("https://assets.codepen.io/5936329/background-code.png"); */
  border: 0px;
  
}

.colorHolder{
  width:100%;
  line-height: 100%;
  content:'Color Palette';
  padding: 10px 0px;
  margin: 0px auto 15px auto;
  cursor:pointer;
  border-radius: 20px;
}

.color{
  height:25px;
  width:25px;
  margin:2px;
  display:inline-block;
  cursor:pointer;
  border-radius: 20px;
}

#ActivityDIV svg{
  width:100%;
  display:inline-block;
  
}

#ActivityDIV svg path{
  cursor:pointer;     
}
</style>
