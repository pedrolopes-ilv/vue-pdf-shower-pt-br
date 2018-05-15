# vue-pdf-shower

## Componente Vue pra exibição de pdfs.
> O componente utilizada a biblioteca PDFJS para buscar e retornar o documento desejado na tela.

## github(Original)
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
            // Url do PDF
            pdfurls: '//cdn.mozilla.net/pdfjs/tracemonkey.pdf',
            // Padrão: 1
            scale: 1.2
        };
    },
    methods: {
        // callback
        onErr(err) {
            console.log('Não foi possível exibir o pdf desejado');
            console.log('Erro：', err);
        }
    }
};
</script>
```
