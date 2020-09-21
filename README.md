# EspoirBao.github.io
my vscode setting.json

```
{
  "emmet.showAbbreviationSuggestions": true,
  "emmet.showExpandedAbbreviation": "always",
  "emmet.includeLanguages": {
    "vue-html": "html",
    "vue": "html",
    "wxml": "html"
  },
}
```
# vue 3 （Vue 3.0 + Ant Design 2.0）

### node_modules

+ package

```
# ant-design
npm i --save ant-design-vue@next
# less
npm install less less-loader --save-dev
# babel
npm install babel-plugin-import --save-dev
```

+ bable.config.js
```
module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset'
  ],
  // 需要添加的内容
  plugins: [
    [
      'import',
      {
        libraryName: 'ant-design-vue',
        libraryDirectory: 'es',
        style: true
      }
    ]
  ]
};
```
+ src/plugins/Ant/index.js
```
import {
    Button,
    Carousel,
    Select ,
  } from "ant-design-vue"
  
  const ant = {
    install(Vue) {
      Vue.component(Button.name, Button);
      Vue.component(Carousel.name, Carousel);
      Vue.component(Select.name, Select);
    }
  }
  
  export default ant

```
+ src/main.js（修改）
```
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import ant from "./plugins/Ant"

createApp(App)
  .use(store)
  .use(router)
  .use(ant)
  .mount('#app')
```
+ vue.config.js（修改）

```
module.exports = {
  css: {
    modules: false,
    loaderOptions: {
      sass: {
      },
      less: {
        lessOptions: {
          javascriptEnabled: true
        }
      }
    }
  }
}
```

+ src/App.vue（修改）
```
<template>
  <a-config-provider :locale="locale">
    <router-view />
  </a-config-provider>
</template>

<script>
import zhCN from 'ant-design-vue/es/locale/zh_CN'
export default {
  data () {
    return {
      locale: zhCN
    }
  }
}
</script>
```

+ Now You Can Use !
```
<template>
  <div class="hello">
    <a-button type="primary">按钮</a-button>
  </div>
</template>
```
