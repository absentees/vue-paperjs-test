<template>
    <div id="app">
        <Canvas :canvas-id="'canvas-one'" ref="childCanvas" />
    </div>
</template>

<script>
import Canvas from "./components/Canvas";

export default {
    name: "App",
    components: {
        Canvas,
    },
    data: () => ({}),
    methods: {
        reset() {
            this.$refs.childCanvas.reset();
        },
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

.canvas-style {
    cursor: crosshair;
    width: 100% !important;
    height: 500px !important;
    border: 5px solid black;
    border-radius: 10px;
    display: block;
    margin: auto;
    box-shadow: 0 10px 8px -8px black;
}
</style>
