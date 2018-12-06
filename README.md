# nuxt-mt

##  Nuxt.js project 

### Nuxt.js + koa2 入门
#### 1. nuxt项目初始化

- 下面是使用 koa 模板方法初始化一个项目，使用该方法需要将 nuxt 的版本降至1.4.2；
- 官方 https://zh.nuxtjs.org/guide/installation 还要提供了脚手架工具，可用使用最新的nuxt2.0版本初始化一个项目。
```js
$ vue init nuxt-community/koa-template <project-name>
$ cd <project-name>
$ npm run dev

<!--
    1. 如果有报错： Plugin/Preset files are not allowed to export objects, only functions
        需要降低nuxt版本至1.4.2：
        npm uninstall nuxt
        npm install nuxt@1.4.2
        
    2.  升级eslint-plugin-html 
        $ npm i eslint-plugin-html@^3
-->
```

#### 2. 新建路由
(创建即配置)在pages目录中新建一个vue文件，即成功创建了路由，文件名也就是路由名称。

#### 3. 模板文件
layouts 目录下的所有文件都属于个性化布局文件，可以在页面组件中利用 layout 属性来引用。
请确保在布局文件里面增加 组件用于显示页面非布局内容。
举个例子：

新建：layouts/search-layout.vue:
```js
<template>
  <div>
    <h1>search page</h1>
    <nuxt/>
    <footer>这是一个自定义的只用于search的模板</footer>
  </div>
</template>
<script>
export default {
}
</script>
```
在 pages/search.vue 里， 可以指定页面组件使用 search-layout 布局。
```js
<template>
  <div>
    <h2>search page</h2>
  </div>
</template>
<script>
export default {
  layout: 'search-layout'
}
</script>  
```
### 4. 全局配置文件：nuxt.config.js
这里面定义了所有页面的head title 和main.css 。。。等

### 5. 接口路由配置
#### 5.1 在server目录新建interface/city.js
```js
import Router from 'koa-router'

const router = new Router({
  prefix: '/city'
})
router.get('/list', async (ctx) => {
  ctx.body = {
    list: ['北京', '上海', '菏泽']
  }
})

export default router
```
#### 5.2 在server/index.js中引入新建路由
```js
import Koa from 'koa'
import { Nuxt, Builder } from 'nuxt'

// 引入新建接口路由
import cityInterface from './interface/city'
// 使用新建接口路由
app.use(cityInterface.routes()).use(cityInterface.allowedMethods())
```
访问：http://localhost:3000/city/list 验证是否成功

#### 5.3 从新建接口中获取数据
```js
<template>
  <div>
    <h2>search page</h2>
    <ul>
      <li v-for="(item, index) in list" :key="index">
        {{item}}
      </li>
    </ul>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  layout: 'search-layout',
  data() {
    return {
      list: ['11', '22', '33']
    }
  },
  async mounted() {
    let res = await axios.get('/city/list')
    console.log(res)
    this.list = res.data.list
  }
}
</script>
```

#### 参考
1. https://github.com/nuxt-community/koa-template
2. https://zh.nuxtjs.org/guide/async-data
3. https://www.jianshu.com/p/840169ba92e6
