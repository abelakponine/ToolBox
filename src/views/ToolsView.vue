<template>
  <div class="d-flex">
    <div id="toolbox-container" class="d-flex w-50 m-auto my-5 p-4">
      <ToolBox/>
    </div>
  </div>
</template>

<style>
  #toolbox-container {
    min-height: 300px;
    border: 1px solid #eaeaea;
    border-radius: 8px;
  }
</style>

<script>
// @ is an alias to /src
import ToolBox from '@/components/ToolBox.vue';

export default {
  name: 'ToolsView',
  components: {
    ToolBox
  },
  methods: {
    formatXml(xml) {
      var formatted = '';
      var reg = /(>)(<)(\/*)/g;
      xml = xml.replace(reg, '$1\\r\\n$2$3');
      var pad = 0;
      
      xml.split('\n').forEach(function (node) {
        var indent = 0;
        if (node.match(/.+<\/\w[^>]*>$/)) {
            indent = 0;
        }
        else if (node.match(/^<\/\w/)) {
            if (pad !== 0) {
                pad -= 1;
            }
        }
        else if (node.match(/^<\w[^>]*[^/]>/)) {
            indent = 1;
        }
        else {
            indent = 0;
        }

        var padding = '';
        for (var i = 0; i < pad; i++) {
            padding += '  '; // Use spaces for indentation
        }
        formatted += padding + node + '\n';
        pad += indent;
      });

      return formatted;
    }
  }
}
</script>
