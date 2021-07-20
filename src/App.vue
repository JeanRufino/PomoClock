<template>
    <div id="body" :class="{ cycle: cycleType.isCycle, break: cycleType.isBreak, longBreak: cycleType.isLongBreak }">
        <img src="./assets/tomato.png" alt="A tomato" />
        <Timer v-model:cycleType="cycleType" />
    </div>
</template>

<script>
import Timer from "./components/Timer.vue";

export default {
    name: "App",
    components: {
        Timer,
    },
    data() {
        return {
            cycleType: {
                isCycle: true,
                isBreak: false,
                isLongBreak: false,
            }
        };
    }
};
</script>

<style>
:root {
    --white: #f0f0f0;
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
#body {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    min-height: 100vh;
    padding: 30px 0;
}
#body.cycle {
    background-color: var(--red);
}
#body.break {
    background-color: var(--blue);
}
#body.longBreak {
    background-color: var(--green);
}
img {
    max-width: 60px;
    max-height: auto;
    padding-bottom: 30px;
}
</style>
