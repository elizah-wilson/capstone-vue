<script setup>
import $ from 'jquery'

(function ($) {

  console.clear()
  console.log('svgColor')
  var mainHolder, colorHolder
  var btnClear
  var svgObject, svgOutline, svgColor
  var swatchUp, swatchDown
  var fillSpeed = 0.15
  var chosenColor = '#FFFFFF'
  var colors = ['#1d561c', '#699b68', '#61ce73', '#afe89a', '#e9edb2', '#efe77b', '#f4d24f', '#bc9d71', '#08316d', '#265a8b', '#5da4ba', '#7ad0d3', '#e7b6af', '#faca9a', '#fe8d7d', '#9b6959', '#552056', '#874a9e', '#b595e5', '#b33a6d', '#e2649e', '#ec8a8e', '#fd6d4a', '#7c373f']
  var closeOffset

  function swatchClick() {
    chosenColor = $(this).data('color')
    console.log(chosenColor)
    TweenMax.to(colorHolder, fillSpeed, { backgroundColor: chosenColor })
  }
  function swatchMove(e) {
    var moveTo = (e.type == 'mouseenter') ? swatchUp : swatchDown;
    TweenMax.to('.swatchHolder', 0.5, moveTo);
  }

  function colorMe() {
    TweenMax.to(this, fillSpeed, { fill: chosenColor });
  }

  function colorRollover(e) {
    var rollover = (e.type == 'mouseenter') ? { scale: 1.2 } : { scale: 1 };
    TweenMax.to($(this), 0.05, rollover);
  }

  function download() {
    var svgInfo = document.querySelector('svg').outerHTML
    var parser = new DOMParser();
    var xmlDoc = parser.parseFromString(svgInfo, "text/xml");
    var dl = document.createElement("a");
    document.body.appendChild(dl); // This line makes it work in Firefox.
    dl.setAttribute("href", "data:image/svg+xml;base64," + xmlDoc);
    dl.setAttribute("download", "coloringpage.svg");
    dl.click();
  }

  function svgRandom() {
    $(svgColor).each(function () {
      var randomNum = Math.floor((Math.random() * colors.length) + 1);
      TweenMax.to(this, fillSpeed, { fill: colors[randomNum] });
    })
  }
  function svgClear() {
    $(svgColor).each(function () {
      TweenMax.to(this, fillSpeed, { fill: "#FFF" });
    })
  }

  $.fn.makeSwatches = function () {
    var swatchHolder = $('<ol/>', { 'class': 'swatchHolder' }).appendTo(this)
    colorHolder = $('<li/>', { 'class': 'colorHolder', 'text': 'Color Palette' }).css('background-color', chosenColor).appendTo(swatchHolder)

    $.each(colors, function () {
      var swatch = $('<li/>').appendTo(swatchHolder)
      $(swatch).css('background-color', this)
      $(swatch).data('color', this)
      $(swatch).on('click', swatchClick)
      $(swatch).on('mouseenter mouseleave', colorRollover)
    })

    var swatchPos = $('.colorHolder').position()
    var swatchHeight = $('.colorHolder').outerHeight(true) + swatchPos.top
    closeOffset = swatchHeight - $('.swatchHolder').outerHeight()

    $('.swatchHolder').on('mouseenter mouseleave', swatchMove)
    $('.swatchHolder').css('bottom', closeOffset)
    swatchUp = { css: { bottom: 0 } }
    swatchDown = { css: { bottom: closeOffset } }
  }
  $.fn.makeSVGcolor = function (svgURL) {
    mainHolder = this
    $(this).load(svgURL, function () {
      svgObject = $('svg', this)
      svgColor = $('g#Color', svgObject).children()
      svgOutline = $('g:nth-child(1)', svgObject).children()
      $(svgColor).on('click', colorMe)
      $(mainHolder).makeSwatches()
      $('.swatchHolder').addClass('gray')
    });
  }

  $.fn.btnClear = function () {
    btnClear = this
    $(btnClear).on('click', svgClear)
  }
}

  (jQuery));

$('#ActivityDIV').makeSVGcolor('https://assets.codepen.io/5936329/Coloringbook1.svg')

$('#btnClear').btnClear()

</script>


<template>

<div class='holder'>
  <div class='Title'>Coloring Page</div>
  
  <div id="imageonly">
    
  </div>
  
  <div class='held' id='ActivityDIV'></div>
 
  <div class='held'>  
    <a id="btnClear"        class="button">Clear Color</a>
  </div>

</div>




</template>


<style scoped>
.holder {
  width: 900px;
  height: 1100px;
  background-image: url("https://assets.codepen.io/5936329/background-code.png");

  border-radius: 35px;
  align-self: center;
  justify-self: center;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  border: 0px solid;

  display: inline-block;

  margin: auto;
  padding: 2vmin;


}

.imageonly {

  background-image: url("https://assets.codepen.io/5936329/background-code.png");

}

.Title {

  font-family: 'Averia Gruesa Libre';
  font-size: 60px;
  text-align: center;
  padding: 30px;
  Color: #b33a6d;


}

.held {
  position: relative;
  overflow: hidden;
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

.buttons {
  display: inline-block;
  padding: 15px;

}

.button {
  transition-duration: 0.4s;
  width: 150px;
  padding: 10px;
  background: #7ad0d3;
  font: 15px Arial, sans-serif;
  height: 40px;
  line-height: 42px;
  border: none;


  outline: none;

  text-align: center;

  border-radius: 40px;
  background: #fff;
  border: px solid #b33a6d;
  color: #b33a6d;
  letter-spacing: 1px;
  text-shadow: 0;

  cursor: pointer;
  transition: all 0.25s ease;
}


.button:hover {
  color: white;
  background: #b33a6d;
}

.btnClear {
  font: 15px Arial, sans-serif;
}

.btnDownloadSVG {
  font: 15px Arial, sans-serif;

}

#ActivityDIV {
  padding: 65px;
  background: white;
  border: 0px;

}

#ActivityDIV .swatchHolder {
  position: absolute;
  bottom: 0px;
  margin: auto;
  left: 0px;
  right: 0px;
  list-style-type: none;
  text-align: center;
  letter-spacing: 1px;
  font-family: Arial;
  display: inline-block;
  padding: 15px;
  width: 260px;
  border-radius: 35px 35px 0px 0px;
  color: #232323;
  background-image: url("https://assets.codepen.io/5936329/background-code.png");
  border: 0px;

}

#ActivityDIV .swatchHolder .colorHolder {
  width: 100%;
  line-height: 100%;
  content: 'Color Palette';
  padding: 10px 0px;
  margin: 0px auto 15px auto;
  cursor: pointer;
  border-radius: 20px;
}

#ActivityDIV .swatchHolder li:not(.colorHolder) {
  height: 25px;
  width: 25px;
  margin: 2px;
  display: inline-block;
  cursor: pointer;
  border-radius: 20px;


}

#ActivityDIV svg {
  width: 100%;
  display: inline-block;

}

#ActivityDIV svg path {
  cursor: pointer;

}
</style>
