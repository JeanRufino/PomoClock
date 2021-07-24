<template>
    <div id="body" :class="{ cycle: isCycle, break: isBreak, longBreak: isLongBreak, 'modal-open': isActive}">
        <header :class="{ cycle: isCycle, break: isBreak, longBreak: isLongBreak }">
            <Menu />
            <img src="./assets/pomodoro.png" alt="A tomato" />
            <TimerOptions 
                v-model:cycleDuration="cycleDuration"
                v-model:breakDuration="breakDuration"
                v-model:longBreakDuration="longBreakDuration"
                v-model:isActive="isActive"
                v-model:maxCycles="maxCycles"
                v-model:maxSets="maxSets"
                v-model:isReset="isReset"
            />
        </header>

        <Timer 
            v-model:cycleDuration="cycleDuration"
            v-model:breakDuration="breakDuration"
            v-model:longBreakDuration="longBreakDuration"
            v-model:isCycle="isCycle"
            v-model:isBreak="isBreak"
            v-model:isLongBreak="isLongBreak"
            v-model:maxCycles="maxCycles"
            v-model:maxSets="maxSets"
            v-model:cycle="cycle"
            v-model:set="set"
        />

        <footer>
            <span>Gostou? Me siga no <a href="https://github.com/JeanRufino?tab=repositories" target="_blank" rel="noopener noreferrer">GitHub</a>!</span> 
            <br>
            <span>Feito com &#9829; por Jean Rufino</span>
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
            isActive: false,
            cycleDuration: 25,
            breakDuration: 5,
            longBreakDuration: 30,
            isCycle: true,
            isBreak: false,
            isLongBreak: false,
            maxSets: 3,
            maxCycles: 4,
            cycle: 1,
            set: 1,
            isReset: false,
        };
    },
    watch: {
        cycleDuration(newVal) { this.cycleDuration = newVal;},
        breakDuration(newVal) { this.breakDuration = newVal; },
        longBreakDuration(newVal) { this.longBreakDuration = newVal; },
        maxSets(newVal) { this.maxSets = newVal; },
        maxCycles(newVal) { this.maxCycles = newVal; },
        isCycle(newVal) { this.isCycle = newVal; },
        isBreak(newVal) { this.isBreak = newVal; },
        isLongBreak(newVal) { this.isLongBreak = newVal; },
        cycle(newVal) { this.cycle = newVal; },
        set(newVal) { this.cycle = newVal; },
        isReset(newVal) { 
            if(newVal == true) {
                this.reset(); 
                this.isReset = false
            }
        }
    },
    methods: {
        reset() {
            this.isCycle = true;
            this.isBreak = false;
            this.isLongBreak = false;
            this.maxSets = 3;
            this.maxCycles = 4;
            this.cycleDuration = 25;
            this.breakDuration = 5;
            this.longBreakDuration = 30;
            this.cycle = 1;
            this.set = 1;
        }
    }
};
</script>

<style>
:root {
    --white: #eeecec;
    --text-black: #29323a;
    --red: #f06666;
    --dark-red: #dd5858;
    --blue: #6fa8e0;
    --dark-blue: #5d7a96;
    --darkest-blue: #4f677e;
    --modal-black: #28313980;
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
    flex-direction: column;
    justify-content: space-between;
    position: relative;
    min-height: 100vh;
    overflow-x: hidden;
    font-family: 'Poppins', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: var(--white);
    font-weight: 500;
    width: 100%;
    z-index: 3;
}
#body.modal-open {
    position: fixed;
    /* width: calc(100% - 17px); */ /* Offset caused by scroll-bar */
    overflow-y: hidden;
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
    display: grid;
    align-items: center;
    justify-content: center;
    grid-template-columns: 1fr calc(32vw + 44px) 1fr;
    width: 100%;
    margin: 0 auto 30px;
    padding: 30px 0 30px 0;
}
header.cycle {
    background-color: var(--dark-red);
}
header.break {
    background-color: var(--dark-blue);
}
header.longBreak {
    background-color: var(--darkest-blue);
}
img {
    max-height: 44px;
    max-width: 44px;
    margin: 0 16vw;
}
/* ---- FOOTER---- */
footer {
    justify-self: flex-end;
    margin-bottom: 30px;
}
footer a {
    font-size: 14px;
    color:var(--white);
    font-weight: 700;
}
footer span {
    font-size: 14px;
}
footer span:not(:last-of-type) {
    margin-bottom: 20px;
}
/* ---- MEDIA QUERIES ---- */
@media screen and (min-width: 500px) {
    footer span,
    footer a {
        font-size: 16px;
        font-size: 16px;
    }
}
</style>
