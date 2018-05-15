<template>
    <div class="pdf-wraper">
        <div id="cvsWraper">
            <div class="loading-pdf" v-if="isloading">{{loadingTxt}}</div>
        </div>
    </div>
</template>
<script>

/**
 * @file pdf viewer
 *
 * @author v_yangtianjiao(v_yangtianjiao@baidu.com)
 */
'use strict';
let PDFJS = require('./dist/pdf.js');
PDFJS.PDFJS.disableWorker = true;
// PDFJS.workerSrc = './dist/pdf.worker.js';
export default {
    name: 'pdf-shower',
    props: {
        pdfurl: {
            default: ''
        },
        scale: {
            default: 1
        }
    },
    data() {
        return {
            pafDoc: null,
            pageNum: 1,
            pageRendering: false,
            pageNumPending: null,
            isloading: true,
            loadingTxt: 'Carregando...'
        };
    },
    mounted() {
        let me = this;
        let cvsWraper = document.getElementById('cvsWraper');
        // Criação do wrapper do pdf
        PDFJS.getDocument(me.pdfurl).then(function (pafObj) {
            me.isloading = true;
            me.pdfDoc = pafObj;
            let totalNum = me.pdfDoc.numPages;

            // Geração do canvas
            for (let i = 1; i <= totalNum; i++) {
                let id = `canvas${i}`;
                let cvsNode = document.createElement('canvas');
                cvsNode.setAttribute('id', id);
                cvsNode.setAttribute('class', 'canvas-item');
                cvsWraper.appendChild(cvsNode);
                me.renderPage(i);

                if (totalNum === i) {
                    me.isloading = false;
                }
            }
        }).catch(function (err) {
            me.loadingTxt = 'Não foi possível carregar o documento solicitado';
            me.$emit('onErr', err);
        });
    },
    methods: {
        // Renderizar número de páginas
        renderPage(num) {
            let me = this;
            let id = `canvas${num}`;
            let canvas = document.getElementById(id);
            let ctx = canvas.getContext('2d');
            me.pageRendering = true;
            // Promise de requisição do número da página
            me.pdfDoc.getPage(num).then(function (page) {
                let viewport = page.getViewport(me.scale);
                canvas.height = viewport.height;
                canvas.width = viewport.width;

                // Renderização do PDF dentro do Canvas
                let renderContext = {
                    canvasContext: ctx,
                    viewport: viewport
                };
                page.render(renderContext);
            });
        }
    }
};
</script>
<style>
#cvsWraper {
    width: 800px;
    margin: 0 auto;
    position: relative;
    overflow-y: scroll;
    height: 450px;
    border: 1px solid #ccc;
}
.canvas-item {
    margin: 0 auto;
}
.loading-pdf {
    text-align: center;
    margin-top: 300px;
}
</style>
