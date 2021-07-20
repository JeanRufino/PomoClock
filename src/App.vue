<template>
    <div id="body" :class="{ cycle: cycleType.isCycle, break: cycleType.isBreak, longBreak: cycleType.isLongBreak }">
        <header>
            <Menu />
            <img src="./assets/pomodoro.png" alt="A tomato" />
            <TimerOptions />
        </header>

        <Timer v-model:cycleType="cycleType" />

        <footer>
            <div>
                <a href="https://www.linkedin.com/in/jean-rufino-40b95113b/" target="_blank" rel="noopener noreferrer">LinkedIn</a>
                <a href="https://github.com/JeanRufino?tab=repositories" target="_blank" rel="noopener noreferrer">GitHub</a>
            </div>
            <span>&copy; 2021 - Jean Rufino</span>
        </footer>
    </div>
</template>

<script>
import Timer from "./components/Timer.vue";
import TimerOptions from "./components/TimerOptions.vue";
import Menu from "./components/Menu.vue";

export default {
    name: "App",
    components: {
        Timer,
        TimerOptions,
        Menu,
    },
    data() {
        return {
            cycleType: {
                isCycle: true,
                isBreak: false,
                isLongBreak: false,
            },
            // periodDurations: {
            //     cycleDuration,
            //     breakDuration,
            //     longBreakDuration,
            // }
        };
    }
};
</script>

<style>
:root {
    --white: #f0f0f0;
    --text-black: #1d2833;
    --red: #ff2d2d;
    --blue: #347fca;
    --dark-blue: #1e4e7e;
    --green: #3cb842;
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
    display: flex;
    position: relative;
    overflow-x: hidden;
    font-family: 'Poppins', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    flex-direction: column;
    min-height: 100vh;
    color: var(--white);
    font-weight: 500;
}
#body.cycle {
    background-color: var(--red);
}
#body.break {
    background-color: var(--blue);
}
#body.longBreak {
    background-color: var(--dark-blue);
}
/* ---- HEADER AND MENUS ---- */
header {
    position: initial;
    display: grid;
    grid-template-columns: 1fr 80px 1fr;
    width: 100%;
    background-color: transparent;
    padding: 30px 0 50px 0;
}
img {
    max-height: 60px;
    max-width: auto;
    margin: 0 auto;
}
footer {
    justify-self: flex-end;
    margin-top: auto;
    padding: 50px 0 30px 0;
}
footer > div {
    margin-bottom: 16px;
}
footer a {
    font-size: 20px;
    color:var(--white)
}
footer a:not(:last-of-type) {
    margin-right: 20px;
}
</style>
