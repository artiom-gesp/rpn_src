<template>
    <div id="app">
        <rpn></rpn>
    </div>
</template>

<script>

    import rpn from '@/components/rpn.vue'

    export default {
        name: 'app',
        components: {
            rpn
        }
    }
</script>

<style>
    body, html
    {
        width: 100%;
        height: 100%;
        margin: 0;

    }
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        background-image: linear-gradient(to right top, #000f23, #004466, #008085, #00ba6b, #a8eb12);
        height: 100%;
    }
</style>
