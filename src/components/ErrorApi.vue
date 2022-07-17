<template>
    <div class="error-api__container">
        Упс! Что-то пошло не так :( Попробуйте перезагрузить страницу
    </div>
</template>

<script>
    export default {
        name: "ErrorApi"
    }
</script>

<style scoped>
    .error-api__container {
        position: absolute;
        top: -50px;
        width: 100%;
        height: 100%;
        background: rgba(156, 8, 8, 0.6);
        color: white;
        text-align: center;
        padding-top: 200px;
        font-size: 30px;
    }
</style>