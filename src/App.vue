<script setup lang="ts">
import { computed, onMounted, reactive, ref, watch } from 'vue';
import data from '../public/data.json'
const width = ref(window.innerWidth)
const height = ref(window.innerHeight)
const canvas = ref<HTMLCanvasElement>()
onMounted(() => {
    function initCanvas() {
        const ctx = canvas.value?.getContext('2d')
        ctx?.arc(width.value / 2, height.value / 2, 300, 0, Math.PI * 2, true)
        ctx?.stroke()
    }
    initCanvas()
    window.addEventListener('resize', () => {
        width.value = window.innerWidth
        height.value = window.innerHeight
        initCanvas()
    })
})
function show(sonvalue: { 'title': string, 'content': string }[]) {
    neirong.show = true
    neirong.main = sonvalue
}
const neirong: { 'show': Boolean, 'main': { 'title': string, 'content': string }[] } = reactive({ 'show': false, 'main': [] })
</script>

<template>
    <canvas :width="width" ref="canvas" :height="height" style="position: fixed;z-index: -10;top: 0;left: 0;"></canvas>
    <div v-if="neirong.show"
        :style="{ width: width * 0.85 + 'px', height: height * 0.85 + 'px', backgroundColor: '#66ccffee', position: 'fixed', left: 0, right: 0, margin: '0 auto', borderRadius: '5%' }">
        <button @click="neirong.show = false"
            style="position: absolute;left: 10px;top: 10px;width: 1.5em !important;height: 1.5em !important; background-color:#ff0000dd;border-radius: 50%;padding: 0;"></button>
        <div style="width: 85%;margin: 0 auto;">
            <div v-for="(value, key) of neirong.main" :key="key"
                style="height: fit-content;border-bottom: black solid 2px;">
                <h3 v-text="value.title"></h3>
                <p v-text="value.content"></p>
            </div>
        </div>
    </div>
    <h1>往日著述</h1>
    <main>
        <div v-for="(value, key, index) of data" :key="index">
            <h2 v-text="key"></h2>
            <div>
                <button @click="show(sonvalue)" v-for="(sonvalue, sonkey, sonindex) of value" :key="sonindex"><strong
                        v-text="sonkey"></strong></button>
            </div>
        </div>
    </main>

</template>
<style scoped>
button {
    width: 4em;
}
</style>
