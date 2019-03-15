<template>
    <div id="app" class="main-box">
         <tu-paper></tu-paper>
    </div>
</template>

<script>
    import TuPaper from './components/paper'
    export default {
        name: 'App',
        components: {
            TuPaper
        }
    }
</script>

<style>
    html, body, .main-box{
        height: 100%;
        width: 100%;
        overflow: hidden;
        margin: 0;
    }

    body{
        background-color: #ebe8e8;
    }

    .tu-btn-bar .el-color-picker__trigger{
        border: 0;
    }
</style>
