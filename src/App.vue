<template>
    <img src="./assets/tomato.png" alt="Vue logo" />
    <Timer />
</template>

<script>
import Timer from "./components/Timer.vue";

export default {
    name: "App",
    components: {
        Timer,
    },
    data() {
        return {};
    },
};
</script>

<style>
:root {
    --text-white: #f0f0f0;
    --text-black: #2c3e50;
    --red: #fa7a52;
    --dark-red: #fa4d31;
    --blue: #76BBFF;
    --green: #94E084;
}
*,
*::after,
*::before {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    position: relative;
}
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    height: 100vh;
    background-color: var(--red);
    /* background-color: var(--blue); */
    /* background-color: var(--green); */
    padding: 30px 0;
}
img {
    max-width: 60px;
    max-height: auto;
    padding-bottom: 30px;
}
</style>
