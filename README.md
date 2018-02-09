# vue-pdf-shower

## 介绍
> 基于pdf.js的pdf简易查看组件。
> 该组件加载全部pdf页面，不提供翻页查看功能。

## github
[vue-pdf-shower](https://github.com/TJ666/vue-pdf-shower)
## install
```
npm i vue-pdf-shower --save
```
## example
```
<template>
    <div>
         <pdfshower 
            :pdfurl="pdfurls" 
            :scale="scale" 
            @onErr="onErr"
        ></pdfshower>
    </div>
</template>

<script>
import pdfshower from 'vue-pdf-shower';
export default {
    name: 'pdfshower',
    components: {
        pdfshower
    },
    data() {
        return {
            // 所查看的pdf url
            pdfurls: '//cdn.mozilla.net/pdfjs/tracemonkey.pdf',
            // 缩放 默认为1
            scale: 1.2
        };
    },
    methods: {
        // 加载失败的callback
        onErr(err) {
            console.log('pdf加载失败，请重试');
            console.log('错误信息：', err);
        }
    }
};
</script>
```