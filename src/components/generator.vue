<template>
    <div class="main">
        <section class="entry">
        <div class="text-entry">
            <label class="text-entry-label">Enter the text, press enter to generate svg</label>
            <input class="text" v-model="text" @change="makeMorse(text)" autofocus/>
        </div>
        <div class="svg-output" ref="svgoutput">
            <svg ref ="svg" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
        	 :width="svgwidth" :height="svgheight" :viewBox="'0 0 '+svgwidth+ ' '+svgheight" :style="'enable-background:new 0 0 '+svgwidth+ ' '+svgheight+';'" xml:space="preserve"
             id="drawing" v-html>
            <g ref="svgmorse" :id="this.text"></g>
            </svg>
        </div>
        </section>
        <div class="export">
            <div  class="settings-mod">
            <h2 @click="settingsopen=!settingsopen"> Tweak <span v-if="settingsopen" >-</span> <span v-if="!settingsopen" >+</span>  </h2>
                <div v-if="settingsopen" class="settings">

                    <div class="settings-item">
                        <label class="settings-label">stroke width</label>
                        <input class="settings-input" v-model="strokew" type="number" /><br>
                    </div>
                    <div class="settings-item">
                        <label class="settings-label">dash width</label>
                        <input class="settings-input" v-model="dashwidth" type="number" /><br>
                    </div>
                    <div class="settings-item">
                        <label class="settings-label">dot width</label>
                        <input class="settings-input" v-model="dotwidth" type="number" /><br>
                    </div>
                    <div class="settings-item">
                        <label class="settings-label">line height</label>
                        <input class="settings-input" v-model="spacewidth" type="number" /><br>
                    </div>
                    <div class="settings-item">
                        <label class="settings-label">dot-dash space</label>
                        <input class="settings-input" v-model="dotdashspace" type="number" /><br>
                    </div>
                </div>
            </div>
            <div  class="copy-mod">
                <h2 @click="exportAsSVG">Download</h2>
                <h2 @click="copyCode">Copy SVG to clipboard</h2>
            </div>
            <div  class="viewsvgcode-mod">
            <h2 @click="viewsvgcode=!viewsvgcode"> View Morse as text <span v-if="viewsvgcode" >-</span> <span v-if="!viewsvgcode" >+</span>  </h2>
                <div v-if="viewsvgcode" class="viewastext">

                    <div class="svgastext" v-html>{{morse}}</div>
                    <input class="hidden" ref="tempcode" type="text" v-model="morse">

                </div>
            </div>
            <div  class="viewastext-mod">
            <h2 @click="viewastext=!viewastext"> View SVG code <span v-if="viewastext" >-</span> <span v-if="!viewastext" >+</span>  </h2>
                <div v-if="viewastext" class="viewastext">

                    <div class="svgastext" v-html>{{svgastext}}</div>
                    <input class="hidden" ref="tempcode" type="text" v-model="svgastext">

                </div>
            </div>

        </div>

    </div>
</template>


<style scoped lang="less">

    .viewastext{}
    .svgastext{
        font-size: 11px;
    }
    .hidden{position: absolute;top:-9999em}
    .text-entry{
        margin-top:40px;
    }


    label{
        font-size:14px;
        text-align:left;
    }
    input.text{
        width:100%;
        font-size:20px;
        // margin:40px 5vw;
        padding:20px;
        box-sizing: border-box;
        outline:none;
    }
    .svg-output{

        height:400px;
        width:100%;
        display: flex;
        justify-content: center;
        align-items: center;
        // border:1px solid red;
        text-align: center;
        margin-top:40px;
        padding:10px;
        background:rgba(125,125,140,.1);

        svg{
            border:1px solid white;
            width:300px;
            height:auto;
            background: white
        }
    }

    h2{
        font-weight:100;
        font-size:16px;
        margin:0;
        line-height: 30px;
        cursor:pointer;

    }
    .settings-mod{
        margin-top:40px;

    }
    .settings{
        font-size: 18px;
        padding:40px;
    }
    .settings-item{}
    .settings-label{}
    .settings-input{
        font-size:18px;
        width:60px;
        border:0;
        text-align: right;
    }

    .morse{
        font-size: 12px;
        min-height:40px;
        /* line-height:40px; */

    }
    // .__line{
    //     stroke-width:2px;
    //     stroke:red;
    //     stroke-linecap:round;
    // }


    .button{
        border:1px solid darkgrey;
        padding:10px 20px;
        font-weight: 300;
        display: inline-block;
    }
</style>

<script>

export default {
  name: 'HelloWorld',
  props: {
    msg: String
},

data(){
    return {

        svgwidth:300,
        svgheight:300,
        viewastext:false,
        viewsvgcode:false,
        settingsopen:true,
        dashwidth : 12,
        svgastext:'no svg',
        dotwidth : 1,
        spacewidth : 10,
        dotdashspace : 10,
        defaultx : 10,
        defaulty : 10,
        yspace : 20,
        strokew : 3,
        style:'.line{stroke:black;stroke-linecap:round;}',
        text:'change the world with the code',
        svgLine:0,
        morse:'',
        svgmorse:'',
        table : {
            'A':'•−',
            'B':'−•••',
            'C':'−•−•',
            'D':'−••',
            'E':'•',
            'F':'••−•',
            'G':'−−•',
            'H':'••••',
            'I':'••',
            'J':'•−−−',
            'K':'−•−',
            'L':'•−••',
            'M':'−−',
            'N':'−•',
            'O':'−−−',
            'P':'•−−•',
            'Q':'−−•−',
            'R':'•−•',
            'S':'•••',
            'T':'−',
            'U':'••−',
            'V':'•••−',
            'W':'•−−',
            'X':'−••−',
            'Y':'−•−−',
            'Z':'−−••',
            'Ä':'•−•−',
            'Á':'•−−•−',
            'Å':'•−−•−',
            'Ch':'−−−−',
            'É':'••−••',
            'Ñ':'−−•−−',
            'Ö':'−−−•',
            'Ü':'••−−',
            '0':'−−−−−',
            '1':'•−−−−',
            '2':'••−−−',
            '3':'•••−−',
            '4':'••••−',
            '5':'•••••',
            '6':'−••••',
            '7':'−−•••',
            '8':'−−−••',
            '9':'−−−−•',
            'apostrophe':'•−−−−•',
            'fs':',•−•−•−',
            'comma':'−−••−−',
            'colon':'−−−•••',
            'question':'••−−••',
            'hyphen':'−••••−',
            'fraction':'−••−•',
            'parentheses':'−•−−•−',
            'quote':'•−••−•',
            'at':'•−−•−•',
            'equals':'-•••−'

        },

    }
},


mounted:function(){

    var style= document.createElement('style');
    style.type = 'text/css';
    style.appendChild(document.createTextNode(this.style))
    this.$refs.svg.appendChild(style);
    this.makeMorse(this.text);

},
methods:{
    makeMorse:function(string){
        // let string = e.target.value;

        this.morse = '';
        this.svgmorse = '';
        this.svgLine = 0;

        for(var i=0;i<string.length;i++){
            let letter = string[i].toUpperCase();

            switch (letter){


            case " ":
                    this.morse+= ' ';
            break;
            case "'":
                    this.morse+= this.toMorse('apostrophe');
            break;
            case ".":
                    this.morse+= this.toMorse('fs');
            break;
            case ",":
                    this.morse+= this.toMorse('comma');
            break;
            case ";":
                    this.morse+= this.toMorse('colon');
            break;
            case "?":
                    this.morse+= this.toMorse('question');
            break;
            case "-":
                    this.morse+= this.toMorse('hyphen');
            break;
            case "/":
                    this.morse+= this.toMorse('parentheses');
            break;
            case "(" || ")":
                    this.morse+= this.toMorse('fraction');
            break;
            case "\"":
                    this.morse+= this.toMorse('quote');
            break;
            case "@":
                    this.morse+= this.toMorse('at');
            break;
            case "=":
                    this.morse+= this.toMorse('equals');
            break;

            default:
                    this.morse+= this.toMorse(letter);
            break;
            }
        }

        this.toSVG(this.morse);

    },

    toSVG:function(str){

        //console.log('toSVG ',str);
        let svgNS = "http://www.w3.org/2000/svg";

        if(str){

            this.$refs.svgmorse.innerHTML='';
            let x = this.defaultx;
            let y = this.defaulty;

            var node = document.createElementNS(svgNS, 'line');
            var group = document.createElementNS(svgNS, 'g');

            this.$refs.svgmorse.appendChild(group);

            for(var i=0;i<str.length;i++){

                var node = document.createElementNS(svgNS, 'line');
                    node.setAttribute('class','line')
                    node.setAttribute('stroke-width',this.strokew)


                let letter = str[i];

                if(letter == '−'){
                    // line = '<line x1="'+x+'" x2="'+(x+dashwidth)+'" y1="'+this.svgLine+'"/>';
                    node.setAttribute('x1',x)
                    node.setAttribute('x2',parseInt(x)+parseInt(this.dashwidth))
                    node.setAttribute('y1',y)
                    node.setAttribute('y2',y)
                    x = parseInt(x)+parseInt(this.dashwidth) + parseInt(this.dotdashspace);

                    group.appendChild(node);

                }
                if(letter == '•'){
                    // line = '<line x1="'+x+'" x2="'+(x+dotwidth)+'" y1="'+y+'"/>';
                    node.setAttribute('x1',x)
                    node.setAttribute('x2',parseInt(x)+parseInt(this.dotwidth))
                    node.setAttribute('y1',y)
                    node.setAttribute('y2',y)
                    x = parseInt(x)+parseInt(this.dotwidth) + parseInt(this.dotdashspace);

                    group.appendChild(node);

                }
                if(letter == ' '){
                    x = this.defaultx;
                    y+= parseInt(this.spacewidth);

                    group = document.createElementNS(svgNS, 'g');
                    this.$refs.svgmorse.appendChild(group);

                }
            }





        }
        else{

        }
        this.updateSVGCode();
    },

        updateSVGCode:function(){
            this.svgastext = this.$refs.svgoutput.innerHTML;
        },

     triggerDownload : function (imgURI,name) {
        var evt = new MouseEvent('click', {
            view: window,
            bubbles: false,
            cancelable: true
        });

        var a = document.createElement('a');
        a.setAttribute('download', name);
        a.setAttribute('href', imgURI);
        a.setAttribute('target', '_blank');

        a.dispatchEvent(evt);

},
    exportAsSVG :  function () {
      var svg = document.querySelector('#drawing');
          //get svg source.
      var serializer = new XMLSerializer();
      var source = serializer.serializeToString(svg);

      //add name spaces.
      if(!source.match(/^<svg[^>]+xmlns="http\:\/\/www\.w3\.org\/2000\/svg"/)){
          source = source.replace(/^<svg/, '<svg xmlns="http://www.w3.org/2000/svg"');
      }
      if(!source.match(/^<svg[^>]+"http\:\/\/www\.w3\.org\/1999\/xlink"/)){
        source = source.replace(/^<svg/, '<svg xmlns:xlink="http://www.w3.org/1999/xlink"');
      }

      //add xml declaration
      source = '<?xml version="1.0" standalone="no"?>\r\n' + source;

      //convert svg source to URI data scheme.
      var url = "data:image/svg+xml;charset=utf-8,"+encodeURIComponent(source);

      this.triggerDownload(url,this.text+'.svg');
      //set url value to a element's href attribute.
      //document.getElementById("link").href = url;
      //you can download svg file by right click menu.


    },
    toMorse:function(letter){
        return this.table[letter];
    },
    copyCode:function(e) {
        var textToCopy = this.$refs.svgoutput; //document.querySelector(e);
        var textBox =  this.$refs.tempcode;
        // textBox.setAttribute('value',textToCopy);

  textBox.select();
  document.execCommand('copy');
}
},

watch: {
    dashwidth: function (val) {
      this.toSVG(this.morse);
    },
    dotwidth: function (val) {
      this.toSVG(this.morse);
    },
    spacewidth: function (val) {
      this.toSVG(this.morse);
    },
    dotdashspace: function (val) {
      this.toSVG(this.morse);
    },
    strokew: function (val) {
      this.toSVG(this.morse);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<!--
Punctuation Mark	Morse
Full-stop (period)	• − • − • −
Comma	− − • • − −
Colon	− − − • • •
Question mark (query)	• • − − • •
Apostrophe	• − − − − •
Hyphen	− • • • • −
Fraction bar	− • • − •
Brackets (parentheses)	− • − − • −
Quotation marks	• − • • − •
At sign	• − − • − •
Equals sign	− • • • −
Error	• • • • • • • •

-->
