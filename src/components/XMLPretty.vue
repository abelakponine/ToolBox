<template>
  <div class="d-flex flex-column m-auto my-1 ms-3 me-3 p-4 bg-dark text-secondary rounded left">
    <h1 class="fs-5 text-primary text-start">Tool Player</h1>
    <p class="text-secondary text-start fs-6">Paste your code in the box below and click `Format!`:</p>
    <div class="d-flex">
      <div class="d-flex flex-column me-4" style="width:40%;">
        <textarea name="" id="xml-input" class="p-4 my-3 border border-secondary rounded" v-model="xmlInput" placeholder="Enter or paste XML codes here..." style="height:300px;"></textarea>
        
        <div class="d-flex">
          <button class="d-flex btn bg-secondary text-white my-2 ms-auto" style="width:max-content;" @click="formatXml(xmlInput)">Format!</button>
          <button class="d-flex btn bg-primary text-white my-2 ms-3" style="width:max-content;" @click="copyFormatted">Copy formatted XML <i v-if="copySpinner">&nbsp;...</i></button>
        </div>
      </div>
      <div v-html="player" style="width:60%;"></div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "XMLPretty",
    data() {
      return {
        xmlInput: '',
        formattedValue: '',
        copySpinner: false
      }
    },
    computed: {
      player() {
        return `
          <p class="p-4 border border-secondary rounded node-text-color" id="player-formatted-content" style="height:450px;overflow-y:auto;outline:none;text-align:left;" contenteditable>
            ${this.formattedValue}
          </p>
        `
      }
    },
    methods: {
      formatXml(xmlString) {
        // var xmlDoc = new DOMParser().parseFromString(xmlString.trim().replaceAll("'",'&#39;').replaceAll(' ','').replaceAll('&nbsp;','').replaceAll("\n", '').replaceAll('&nbsp;', ''), 'application/xml');
        // var serializer = new XMLSerializer();
        // var prettifiedXML = serializer.serializeToString(xmlDoc);
        var prettifiedXML = xmlString.trim().replaceAll("'",'&#39;').replaceAll("    ", '').replaceAll("> <", '><').replaceAll('&nbsp;','')
          .replaceAll("\n", '').replaceAll('&nbsp;', '');

        // Add proper indentation
        var formattedXML = '';
        var reg = /(>)(<)(\/*)/g;
        prettifiedXML = prettifiedXML.replace(reg, '$1\r\n$2$3');
        var pad = 0;
        
        prettifiedXML.split('\r\n').forEach(function (node) {
          
          var indent = 0;
          if (node.match(/.+<\/\w[^>]*>$/)) {
              indent = 0;
          } else if (node.match(/^<\/\w/)) {
              if (pad !== 0) {
                  pad -= 1;
              }
          } else if (node.match(/^<\w[^>]*[^/]>.*$/)) {
              indent = 1;
          } else {
              indent = 0;
          }
          
          var padding = '';
          for (var i = 0; i < pad; i++) {
              padding += '&nbsp;&nbsp;&nbsp;&nbsp;';
          }
          
          node = node.replaceAll('>', '&#62;');
          node = node.replaceAll('<', '&#60;');
          node = node.replaceAll('&#60;', '<span class="node-color">&#60;');
          node = node.replaceAll('&#60;/', '</span>&#60;/');
          node = node.replaceAll('&#62;', '&#62;</span>');
          node = node.replaceAll('&#60;/', '<span class="node-color">&#60;/');
          node = node.replaceAll('&#62;', '&#62;</span>');
          // node = node.replaceAll('!#',' ')
          
          formattedXML += padding + node + '<br/>';
          pad += indent;
        });
        
        // console.log(formattedXML)
        this.formattedValue = formattedXML;
        return formattedXML;
      },
      copyFormatted() {
        
        this.copySpinner = true;

        setTimeout(()=>{
          this.copySpinner = false;

          var element = document.querySelector("#player-formatted-content");
          // select formatted
          var range = document.createRange();
          range.selectNodeContents(element);
          var selection = window.getSelection();
          selection.removeAllRanges(); // Clear existing selection
          selection.addRange(range);
          // copy formatted
          navigator.clipboard.writeText(element.innerText.replaceAll(String.fromCharCode(160)," "));
          console.log("Copied!")
        }, 500)
      }
    }
  }
</script>

<style>
.node-color {
  color: #2091cc;
}
.node-text-color {
  color: #e0a24d;
}
#player-formatted-content::selection, #player-formatted-content *::selection {
  background: #2e3031;
}
</style>
