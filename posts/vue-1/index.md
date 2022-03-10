# Vue基础入门



入门Vue!
<!--more-->

## 脚手架创建项目

```bash
vue create vue-el-plus-demo
npm run server
```

## 快速删除node_modules

```bash
# 安装插件（已经安装，不用反复依赖）
npm install rimraf -g
# 切到到工程目录下
rimraf node_modules
# 等到删除即可
```



## 使用vite创建vue3-js项目

```bash
npm init vite@latest my-vue-app --template vue
npm install 
```

## 路由

### 安装

```bash
npm i --save vue-router@next
```

### router.index.js

```js
import { createRouter, createWebHistory } from 'vue-router'


const routes = [
  {
    path: '/',
    name: 'Home',
    component: () => import('../views/Home.vue')
  },
  {
    path: '/login',
    name: 'Login',
    component: () => import('../views/LoginRegister.vue')
  },
  {
    path: '/:catchAll(.*)',
    name: '/404',
    component: () => import('../views/404.vue')
  }
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```

## 解决WebStorm识别不到“@”符号路径问题

```json
{
  "compilerOptions": {
    "target": "es6",
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  },
  "exclude": ["node_modules", "dist"],
  "include": ["src/**/*"]
}
```

## reset.css



## 安装scss

```bash
npm install -g sass
```

### 弹窗

```vue
<script>
import {reactive} from "vue/dist/vue";
import {ElMessage} from "element-plus";

export default {
  name: 'App',
  setup() {
    let data = reactive({
      name: '范坚强',
      obj: {
        age: 28
      }
    })
    function handleTab() {
      ElMessage('this is a message.')
    }

    return {
      data,
      handleTab
    }
  }
}
</script>
```

### vite导入组件

```js
// 需要使用拓展名
import navbar from '@/views/navbar.vue'
```

# 构建项目流程

## 安装yarn

```bash
npm install -g yarn
```

## 安装依赖(deprecated)

```bash
npm init vite-app project-name # 初始化项目
cd project-name
yarn # 安装依赖
npm i -S typescript vue-router@next # 安装ts支持和router
yarn add element-plus
```

## 安装依赖

```bash
# vite@latest 是正式版
npm init vite@latest project-name
cd project-name
yarn
npm i -S typescript vue-router@next # 安装ts支持和router
yarn add element-plus
yarn add @element-plus/icons

npm install @element-plus/icons-vue
```

## 配置ts

1. `npx tsc --init` 创建`tsconfig.json`文件

2. 把根目录下的`main.js`文件改名为`main.ts`

3. 把根目录下的`index.html`中引入的`main.js`改名为`main.ts`

4. 同时把`.vue`文件里的`<script>`标签中加入`lang="ts"`

5. 在项目根目录创建`shim.d.ts`文件，同时在其中写入以下代码，用于配置ts支持识别`.vue`文件

   ```typescript
   declare module "*.vue" {
   	import { Component } from "vue";
   	const component: Component;
   	export default component;
   }
   ```

## 配置router：

1. 在`src`下建立`router`目录并在其中创建`index.ts`文件，并在其中写入（此处的地址为自己在根目录创建`views`文件夹下创建`index.vue`文件，可根据自己需要创建）

   ```typescript
   import { RouteRecordRaw, createRouter, createWebHistory } from "vue-router";
   
   const routes: RouteRecordRaw[] = [
     {
       path: "/",
       name: "Home",
       component: import("../views/index.vue"),
     },
   ];
   
   const router = createRouter({
     history: createWebHistory(process.env.BASE_URL),
     routes,
   });
   
   export default router;
   ```

2. 修改`main.ts`文件引入`vue-router`

   ```typescript
   import { createApp } from "vue";
   import App from "./App.vue";
   import router from "./router/index";
   
   createApp(App).use(router).mount("#app");
   ```

## TS 导出组件

### 安装依赖

```
yarn add vue-property-decorator
```

### 加载组件

```typescript
import {Component, Vue} from 'vue-property-decorator'
import MenuBar from ...
@Component({
  components: {
    MenuBar
  },
})
export default class App extends Vue {}
```

## ts父子组件传值

```typescript
Array 可以写成 Array as PropType<oneType[]>

Object 可以写成 Object as PropType<oneType>

然后你可以定义你的type oneType = {}

PropType 从vue 导入
```

# Tips

路由使用提前加载,如果使用按需加载会出现闪屏

提前加载

```ty
import about from '../view/about.vue'

{
	path: '/about',
	name: 'about',
	component: about,
}
```

按需加载

```types
{
	path: '/about',
	name: 'about',
	component: ()=>import("../view/about.vue"),
}
```


