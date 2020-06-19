<template>
  <div class="jsonformat">
    <div class="jsonformat-head">
      <div class="jsonformat-head__title">JSON格式化</div>
    </div>
    <div class="jsonformat-body">
      <a-textarea
        v-model="input"
        class="jsonformat-body__input"
        placeholder="Basic usage"
        :rows="25"
      />
      <pre
        class="jsonformat-body__output"
        :class="isWrong"
      ><code class="lang-html" v-html="display"></code></pre>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import hljs from 'highlight.js/lib/core'
import javascript from 'highlight.js/lib/languages/javascript'
import 'highlight.js/styles/github.css'

const jsonlint = require('json-lint')

export default Vue.extend({
  components: {},
  data() {
    return {
      input: '',
      isWrong: ''
    }
  },
  computed: {
    display() {
      return this.format(this.input)
    }
  },
  methods: {
    format(str) {
      if (!str) return ''
      const error = jsonlint(str)
      this.isWrong = ''
      if (error.error) {
        this.isWrong = 'is-wrong'
        return error.error
      }
      hljs.registerLanguage('javascript', javascript)
      return hljs.highlight(
        'javascript',
        JSON.stringify(JSON.parse(str), null, 2)
      ).value
    }
  },
  head() {
    return {
      meta: [
        {
          hid: 'keywords',
          name: 'keywords',
          content:
            'hellojson.com 在线工具,在线Json格式化,jsbin,Json格式化,JavaScript工具,Json'
        }
      ]
    }
  }
})
</script>

<style lang="postcss">
@reset-global pc;
@nasp jsonformat {
  @b head {
    height: 50px;
    line-height: 50px;
    text-align: center;
    background-color: #006cbf;
    box-shadow: 0px 1px 4px #000;
    @e title {
      display: inline-block;
      font-size: 20px;
      background: linear-gradient(to right, #fff, #40a9ff);
      -webkit-background-clip: text;
      color: transparent;
    }
  }
  @b body {
    padding: 50px;
    display: flex;
    @e input {
      width: 0;
      flex: 1;
      margin-right: 50px;
    }
    @e output {
      overflow-x: auto;
      flex: 1;
      border: 1px solid #d9d9d9;
      border-radius: 4px;
      min-height: 32px;
      padding: 4px 11px;
      font-size: 16px 24px;
      margin-bottom: 0;
      @s wrong {
        white-space: pre-wrap; /* css-3 */
        word-wrap: break-word; /* Internet Explorer 5.5+ */
        color: red;
      }
    }
  }
}
</style>
