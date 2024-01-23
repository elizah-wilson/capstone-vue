<script setup>
import $ from 'jquery'
import router from '@/router';
import { ref, onMounted } from 'vue'
import Header from '../components/Header.vue'

let prompt = ref()
let response = ref('')

function currentDate() {
  const current = new Date();
  const date = `${current.getMonth() + 1}-${current.getDate()}-${current.getFullYear()}`;
  return date
}

function currentDatePage() {
    const current = new Date();
    const date = `${current.getMonth() + 1}/${current.getDate()}/${current.getFullYear()}`;
    return date;
}

//fetchs quote of the day from my api
function fetchPrompt() {
  fetch("http://localhost:3000/daily-quote")
    .then((response) => {
      return response.text()
    })
    .then((quote) => {
      prompt.value = quote
    })
}
fetchPrompt()

// post SVG file to aws s3 bucket
function uploadSVG(svgInfo) {

  //sends svg info to backend, which then puts svg info into object inside of s3 bucket
  fetch("http://localhost:3000/upload/",
    {
      headers: { "Content-Type": "application/json", "Authorization": document.cookie },
      // curly brackets implies that element it is an object, so looking for key value pairs
      body: JSON.stringify({ "svgInfo": svgInfo, "prompt": prompt.value, "response": response.value }),
      method: "PUT"
    })
    .then(response => {
      if (response.status === 201) {
        alert("You will be directed to the Feed Wall, where you can find your post!")
        router.push("/feed")
      } else {
        alert("Something went wrong!")
      }
    })
    .catch(error => {
      console.log(error)
    })

  
}

onMounted(() => {
  (function ($) {
    var mainHolder, colorHolder
    var btnRandom, btnClear, element
    var svgObject, svgOutline, svgColor
    var swatchUp, swatchDown
    var fillSpeed = 0.15
    var chosenColor = '#FFFFFF'
    var colors = ['#FFFFFF', '#8E53A1', '#6ABD46', '#71CCDC', '#F7ED45', '#F7DAAF', '#EC2527', '#F16824', '#CECCCC', '#5A499E', '#06753D', '#024259', '#FDD209', '#7D4829', '#931B1E', '#B44426', '#979797', '#C296C5', '#54B948', '#3C75BB', '#F7ED45', '#E89D5E', '#F26F68', '#F37123', '#676868', '#9060A8', '#169E49', '#3CBEB7', '#FFCD37', '#E5B07D', '#EF3C46', '#FDBE17', '#4E4D4E', '#6B449B', '#BACD3F', '#1890CA', '#FCD55A', '#D8C077', '#A62E32', '#F16A2D', '#343433', '#583E98', '#BA539F', '#9D2482', '#DD64A5', '#DB778D', '#EC4394', '#E0398C', '#68AF46', '#4455A4', '#FBEE34', '#AD732A', '#D91E36', '#F99B2A']
    var closeOffset

    function swatchClick() {
      chosenColor = $(this).data('color')
      TweenMax.to(colorHolder, fillSpeed, { backgroundColor: chosenColor })
    }

    // makes color palette move up when mouse hovers over it 
    function swatchMove(e) {
      var moveTo = (e.type == 'mouseenter') ? swatchUp : swatchDown;
      TweenMax.to('.swatchHolder', 0.5, moveTo);
    }

    // fills selected element with chosen color and fills it at 0.15 speed 
    function colorMe() {
      TweenMax.to(this, fillSpeed, { fill: chosenColor });
    }

    function colorRollover(e) {
      var rollover = (e.type == 'mouseenter') ? { scale: 1.2 } : { scale: 1 };
      TweenMax.to($(this), 0.05, rollover);
    }

    // grabs svg file
    // change func to get svg tag
    function saveSVG() {
      // grabbing the tag and the content inside it
      var svgInfo = document.querySelector('svg').outerHTML
      // calls function that makes the put request to aws s3 server 
      let svgInfoWHeader = `<?xml version="1.0" encoding="iso-8859-1"?> <!-- Uploaded to: SVG Repo, www.svgrepo.com, Generator: SVG Repo Mixer Tools --><!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">` + svgInfo
      uploadSVG(svgInfoWHeader)
    }

    function svgDownloadSVG() {
      var svgInfo = $(svgObject).clone();
      console.clear()
      console.log(svgInfo)
      $(this).attr({
        href: "data:image/svg+xml;base64," + svgInfo.toString(),
        download: 'coloringBook.svg',
        target: "_blank"
      });
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
      colorHolder = $('<li/>', { 'class': 'colorHolder', 'text': 'Current Color' }).css('background-color', chosenColor).appendTo(swatchHolder)

      swatchHolder.addClass('swatchHolder')
      colorHolder.addClass('cardHolder')

      $.each(colors, function () {
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
      $('.swatchHolder').css('bottom', closeOffset)
      swatchUp = { css: { bottom: 0 } }
      swatchDown = { css: { bottom: closeOffset } }
    }

    $.fn.makeSVGcolor = function (svgURL) {
      mainHolder = this

      // loads svg object into the mainHolder from a object url 
      $(this).load(svgURL, function () {
        // svgObject is the svg element <svg></svg>
        svgObject = $('svg', this)
        //svgColor applies the id=Color to onto the svg's g elements, which allow them the paths inside of them to be colored in
        svgColor = $('g#Color', svgObject).children()
        // selects the outer g element, which has the paths of the black outline of the svg
        svgOutline = $('g:nth-child(1)', svgObject).children()
        $(svgColor).on('click', colorMe)
        $(mainHolder).makeSwatches()
        $('.swatchHolder')
      });
    }

    // applys the svgRandom functionality to the this element
    $.fn.btnRandom = function () {
      btnRandom = this
      $(btnRandom).on('click', svgRandom)
    }
    // applys the svgClear functionality to the this element
    $.fn.btnClear = function () {
      btnClear = this
      $(btnClear).on('click', svgClear)
    }

    // applies download functionality to the 
    $.fn.btnShowcase = function () {
      element = this
      $(element).on('click', saveSVG)
    }

    $('#ActivityDIV').makeSVGcolor(`https://mysvgfiles.s3.us-east-2.amazonaws.com/${currentDate()}/0.svg`)
    $('#btnRandom').btnRandom()
    $('#btnClear').btnClear()
    // $('#btnDownloadSVG').btnDownload()
    $('#btnShowcase').btnShowcase()
  }(jQuery));


})
</script>


<template>
  <Header id="header"></Header>
  <div class='holder'>
    <div class='Title'>PAGE OF THE DAY {{ currentDatePage() }}</div>
    <div class='bttns-container' id='ActivityDIV'></div>
    <div class='bttns-container'>
      <a id="btnRandom" class="button">Random Color</a>
      <a id="btnClear" class="button">Clear Color</a>
      <a id="btnShowcase" class="button gray">Share</a>
    </div>
    <div id="prompt-container">
      <p id="prompt"> "{{ prompt }}" </p>
      <textarea id="prompt-response" placeholder="Write your musings here..." maxlength="280" v-model="response"></textarea>
    </div>
  </div>
</template>


<style>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;

}

#prompt-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: white;
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
}

#prompt {
  font-size: larger;
  font-weight: bold;
  text-align: center;
  color:  #0B5D1E;
}

#prompt-response {
  background-color:rgba(208, 241, 191, 1);
  color: #0B5D1E;
  padding: 10px;
  width: 500px;
  height: 100px;
  font-size: larger;
  border-color: rgba(208, 241, 191, 1);
}

.holder {
  width: 900px;
  background-color: rgba(182, 215, 185, 1);
  align-self: center;
  justify-self: center;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  border: 0px solid;
  display: inline-block;
  padding: 2vmin;
  margin-bottom: 10px;
}

.Title {
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  font-size: 25px;
  text-align: center;
  margin-bottom: 10px;
  color:  #0B5D1E;
}

.bttns-container {
  position: relative;
  overflow: hidden;
  border-radius: 20px;
  align-self: center;
  justify-self: center;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  border: 0px;
  margin-top: 10px;
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

#ActivityDIV {

  background: white;
  border: 0px;
  height: 690px;
  border-radius: inherit;

}

.swatchHolder {
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
  background-color: white;
  border: 0px;

}

.colorHolder {
  width: 100%;
  line-height: 100%;
  content: 'Color Palette';
  padding: 10px 0px;
  margin: 0px auto 15px auto;
  cursor: pointer;
  border-radius: 20px;
}

.color {
  height: 25px;
  width: 25px;
  margin: 2px;
  display: inline-block;
  cursor: pointer;
  border-radius: 20px;
}

#ActivityDIV svg {
  width: 100%;
  height: 100%;
}

#ActivityDIV svg path {
  cursor: pointer;
  width: 100%;
  height: 100%;
}
</style>
