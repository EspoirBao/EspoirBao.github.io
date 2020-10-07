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


# zao vue.vue.config.js 
```
const path = require('path');
const TerserPlugin = require('terser-webpack-plugin') 
function resolve(dir) {
  return path.join(__dirname, dir)
}
module.exports = {
  publicPath: './',
  lintOnSave: true,//启用lint保存
  productionSourceMap: false,//禁用SourceMap
  chainWebpack: (config) => {
    config.resolve.alias//配置别名
      .set('@$', resolve('src'))
      .set('assets', resolve('src/assets'))
      .set('components', resolve('src/components'))
      .set('static', resolve('src/static'))

  },
  devServer: {//开发环境服务器代理配置
    proxy: {
      '/zao': {
        target: 'https://isdk.zaoyx.com',
        changeOrigin: true,
        pathRewrite: {
          '^/zao': ''
        }
      },
      '/local': {
        target: 'http://app.9hplay.com',
        changeOrigin: true,
        pathRewrite: {
          '^/local': ''
        }
      }
    }
  },
  configureWebpack: {//webpack配置
    optimization: {
      minimizer: [
        new TerserPlugin({
          terserOptions: {
            ecma: undefined,
            warnings: false,
            parse: {},
            compress: {
              drop_console: true,
              drop_debugger: false,
              pure_funcs: ['console.log'] // 移除console
            }
          },
        }),
      ]
    }
  },
}
```

