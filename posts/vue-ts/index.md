# Vue-TS入门



Vue + TS + element-plus + Vite 搭建管理项目
<!--more-->

# 前言:

由于平台新存储了大量接口取来的数据,因此需要搭建一个数据管理平台

前端在技术上硬着头皮选了`Vue3`和`TS`

经过几天的`emotional damage`终于算入一点门了

开个专题在这里记录一下学习(踩坑)经历

本项目使用`element-plus`+`vue3`+`typescript`+`vite`搭建

# 使用element-plus

## npm安装

```bash
npm install element-plus --save
```

## main.ts

```typescript
import ElementPlus from "element-plus";
import "element-plus/dist/index.css";
import * as Icons from "@element-plus/icons-vue";

const app = createApp(App);
app.use(ElementPlus);
app.use(router).mount("#app");

// 全局引入element-plus icon
Object.keys(Icons).forEach((key) => {
  app.component(key, Icons[key as keyof typeof Icons]);
});
```

## 组件使用

### 首页搭建

首页由`header`,`navbar`,`router-viewer`三部分构成

使用`element-plus`的`el-menu`和`el-row`创建`navbar`和`header`

### `index`页面引入`navbar`和`header`

利用`element-plus`的容器`el-container`来布局

```vue
<template>
  <el-container style="height: 100%; border: 1px solid #eee">
    <el-header height="85px" style="background-color:#F5F5F5;">
      <com_header></com_header>
    </el-header>
    <el-container>
      <el-aside width="230px" style="background-color:#F5F5F5">
        <com_navbar></com_navbar>
      </el-aside>
      <el-main>
        <router-view/>
      </el-main>
    </el-container>
  </el-container>
</template>

<script lang="ts" setup>
import com_navbar from '../components/com_navbar.vue'
import com_header from '../components/com_header.vue'
</script>
```

### 父子组件传值

在编写一个显示数据的表格页面时,将`table`写到了一个组件中,并且在页面中引入了该组件

数据在页面中请求得到,需要赋值给子组件,出现了父子组件传值的问题

不得不说...vue的文档写的让我真是头大...感觉是给前端大佬看的,初学前端不适合看

其实实现起来很简单,只需要声明一个`props`,在我看来就是子组件暴露给外部的一个变量

#### 子组件

```vue
<template>
	<el-table :data="props.tableData" style="width: 100%">
        ...
    </el-table>
</template>

<script lang="ts" setup>
interface inter {
  zh_name: string;
  name: string;
  url: string;
  args: string;
  comment: string;
  request: string;
}
// 定义Props
const props = defineProps<{
  tableData: Array<inter>;
}>();
    
// 通过 props.tableData 获取该变量的值
</script>
```

#### 父组件

```vue
<template>
	<com_table v-model:tableData="fatherData"></com_table>
</template>

<script lang="ts" setup>
import com_table from "../../components/com_table.vue";
import axios from "axios";
import {
  ref,
} from "vue";

// 创建变量
const fatherData = ref([]);
// 请求数据
function getdata(): any {
  axios
    .post("interface/get_list")
    .then((res) => {
      fatherData.value = res.data;
    })
    .catch((err) => {
      console.log(err);
    });
}
getdata();
</script>
```

## `icon`使用变量定义

```vue
<el-icon>
    <component :is="item.icon"></component>
</el-icon>
```

# `<script lang="ts" setup>`语法糖

在vue中，`sfc(单文件组件)`指的是文件后缀名为`.vue`的特殊文件格式，它允许将 Vue 组件中的模板、逻辑 与 样式封装在单个文件中

## 标准的sfc写法

在使用TS的情况下，标准的sfc需要借助defineComponent来进行类型推断

```vue
<template>
  <div>
    parent{{count}}
  </div>
</template>

<script lang="ts">
  import { defineComponent } from 'vue'
  
  export default defineComponent({
    setup() {
      return {
        // 暴露给template的属性
      }
    }
  })
</script>
```

script setup的推出，目的是为了让开发者更高效率的开发组件，减少样板内容，减轻开发负担。仅仅需要给script标签添加一个setup属性，就能将script变成setup函数，同时定义的变量，函数，导入的组件都会默认暴露给模板

## 暴露变量

### 未使用语法糖

```vue
<script>
import { defineComponent, ref} from 'vue'

export default defineComponent({
  setup() {
    const count = ref(0)
    return {
      count
    }
  }
})
</script>
```

### 使用语法糖

```vue
<template>
  <div>
    parent{{count}}
  </div>
</template>

<script setup lang="ts">
import {ref} from 'vue'
const count = ref(0)
</script>
```

## 组件挂载

### 未使用语法糖

```vue
<template>
  <div>
    parent
    <child />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import child from './childComponent'

export default defineComponent({
  components: {
    child
  },
  setup() {
    // ...
  }
})
</script>
```

### 使用语法糖

```vue
<template>
  <div>
    parent
    <child />
  </div>
</template>

<script setup lang="ts">
import child from './childComponent'
</script>
```

## Props

### 未使用语法糖

#### 父组件

```vue
// parent.vue
<template>
  <child :count={count} />
</template>

<script lang="ts">
impor {defineComponent,ref} from 'vue'
import child from './childComponent'

export default defineComponent({
  components: {
    child
  },
  setup() {
    const count = ref(0)
    return {
      count
    }
  }
})
</script>
```

#### 子组件

```vue
// child.vue
<template>
  child{{count}}
</template>

<script lang="ts">
impor {defineComponent} from 'vue'
export default defineComponent({
  props: ['count'],
  setup(props) {
    return {}
  }
})
</script>
```

### 使用语法糖

#### 父组件

```vue
// parent.vue
<template>
  <child :count={count} />
</template>

<script setup lang="ts">
impor {ref} from 'vue'
import child from './childComponent'
const count = ref<number>(0)
</script>
```

#### 子组件

```vue
// child.vue
<template>
  child{{count}}
</template>

<script setup>
defineProps(['count'])
</script>
```

### props赋予默认值

withDefaults会对默认值进行类型检查

```vue
<script lang="ts" setup>
  
interface d {
  name: string  
}

interface Props {
  count: number; // Number要换成ts的语法
  ---title:  string;
  data: d;
}

const props = withDefaults(defineProps<Props>(), {
  count: 0,
  ---title:  'header',
  data: () => ({name: '王小二'})
})
</script>
```

## defineProps

```vue
<template>
  <h1>{{ msg }}</h1>
  <div @click="clickThis">1111</div>
</template>
```

### ts专有声明-无默认值

```vue
<script setup lang="ts">
  defineProps<{
    msg: string,
    num?: number
  }>()
</script>

// 多值传值
<script setup lang="ts">
  defineProps<{ title?: string, hello?: (string | number) }>()
</script>
```

### ts专有声明-有默认值

```vue
<script setup lang="ts">
    interface Props {
        msg?: string
        labels?: string[]
    }
    const props = withDefaults(defineProps<Props>(), {
        msg: 'hello',
        labels: () => ['one', 'two']
    })
</script>
```

### 非ts专有声明

```vue
<script setup lang="ts">
    defineProps({ // 非ts专有声明
    msg: String,
    num: {
      type:Number,
      default: ''
    }
  })
</script>
```

## defineExpose

在标准组件写法里，子组件的数据都是默认隐式暴露给父组件的，但在 script-setup 模式下，所有数据只是默认 return 给 template 使用，不会暴露到组件外，所以父组件是无法直接通过挂载 ref 变量获取子组件的数据

如果要调用子组件的数据，需要先在子组件显示的暴露出来，才能够正确的拿到，这个操作，就是由 expose 来完成

defineExpose API 来实现 expose 的功能

### 子组件暴露属性

```vue
<template>
  <div>子组件helloword.vue</div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
const count = ref(123456)
defineExpose({
  count //暴露 count 变量
})
</script>
```

### 父组件获取属性

```vue
<template>
  <div @click="helloClick">父组件</div>
  <helloword ref="hello"></helloword>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import helloword from './components/HelloWorld.vue'
const hello = ref(null)
const helloClick = () => {
  console.log(hello.value.count) // 123456
}
</script>
```

父组件可以通过 ref API 去拿到子组件暴露出来的数据

通过`ref="hello"`获得子组件实例

从而通过`hello.value.count`获取暴露的变量

## defineEmits

### 父组件

```vue
<template>
  <div >
    <page_child @nextStep="father_click"></page_child>
  </div>
</template>

<script setup lang="ts">
import page_child from '@/view/view_test/page_child.vue'
const father_click = ($event: any) => {
    console.log($event);
}
</script>
```

### 子组件

```vue
<template>
  <div>
    <p><button @click="nextDown">子按钮</button></p>
  </div>
</template>

<script lang="ts" setup>
    const emit= defineEmits(['nextStep'])
    const nextDown = () => {
    	emit('nextStep',10)
  	}
</script>
```



# vite启用跨域访问

## vite.config.ts

```typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// 查看文档
// https://cn.vitejs.dev/config

export default defineConfig({
  plugins: [vue()],
  server: {
    proxy: {
      '/api': {
        target: 'http://127.0.0.1:8080',
        changeOrigin: true,
        //rewrite: (path) => path.replace(/^\/api/, '')
      },
    }
  }
})
```

其中`/api`指向接口网址

# 使用`alias:@`路径

使用`@`路径会大大节省脑子,否则全是相对路径...人直接傻了

修改 `vite.config.ts` 和 `tsconfig.json`

## vite.config.ts

```ts
export default defineConfig({
  plugins: [vue()],
  alias:{
    '@':'/src/',
    '@components':'/src/components/',
    '@assets':'/src/assets/',
  }
})
```

## tsconfig.json

配完后可以直接使用，但是编辑器会标红，需要修改 `tsconfig.json`

加上 `baseUrl` 跟 `paths`

```json
{
  ...
  "compilerOptions": {
    ...
    "baseUrl": "./",
    "paths": {
      "@/*": ["./src/*"],
      "@components/*": ["./src/components/*"],
      "@assets/*": ["./src/assets/*"]
   }
  },
  ...
}
```

## 使用`path`

在 `vite.config.ts` 中，如果使用 `__dirname + path` 方式写地址的话，如果 TS 报错提示找不到，则需要安装 `@types/node`

```shell
yarn add @types/node
```

vite.config.ts

```ts
import { resolve } from 'path'

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': resolve(__dirname, 'src'),
      '@assets': resolve(__dirname, 'src/assets')
    }
  }
})
```

`tsconfig.json`照常加上 `baseUrl` 跟 `paths`

# TS模块化

## 使用JS自带库

当TS在使用JS中自带的window时,会发生报错,需要额外声明才能使用

```typescript
// 使用 window.webkitRTCPeerConnection 以及 window.mozRTCPeerConnection
declare global {
  interface Window {
    webkitRTCPeerConnection: any;
    mozRTCPeerConnection: any;
  }
};
```

## 引入外部库

例如引入外部`js`文件

需要更改`index.html`

```html
<body>
    <div id="app"></div>
    <script type="module" src="/src/main.ts"></script>
    <script src="https://pv.sohu.com/cityjson?ie=utf-8" ></script>
</body>
```

使用引入文件的参数,需要`declare`

```typescript
declare let returnCitySN:any; // returnCitySN为参数

export function getIP(){
  return returnCitySN["cip"];
}
```

## 引入模块化函数

```typescript
export function function_name(){
    return something;
}
```




