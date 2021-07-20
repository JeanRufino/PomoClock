<template>
    <button @click="toggleOptions">
        <span class="material-icons-outlined">
            settings
        </span>
    </button>
    <div class="options" :class="{ active: isActive }">
        <label for="cycleDuration">Duração do ciclo</label>
        <input id="cycleDuration" type="range">
        <input id="cycleDuration" type="number">

        <label for="breakDuration">Duração do descanço</label>
        <input id="breakDuration" type="range">
        <input id="breakDuration" type="number">

        <label for="longBreakDuration">Duração do descanço longo</label>
        <input id="longBreakDuration" type="range">
        <input id="longBreakDuration" type="number">
    </div>
</template>

<script>
export default {
    name: 'TimerOptions',
    props: {

    },
    data() {
        return {
            isActive: true,
        }
    },
    methods: {
        toggleOptions() {
            this.isActive ? this.isActive = false : this.isActive = true;  
        }
    }
}
</script>

<style scoped>
    .options {
        position: absolute;
        right: 0;
        top: 0;
        width: 100%;
        height: 100%;
        padding: 10vh 0; 
        background-color: var(--red);
        transition: 400ms ease-out;
        transform: translateX(100%);
        z-index: 2;
    }
    .options.active {
        transform: translateX(0);
    }
    button { 
        z-index: 3;
        border: none;
        background-color: transparent;
        width: max-content;
        align-items: center;
        margin-left: auto;
        margin-right: 30px;
        cursor: pointer;
    }
    button span {
        color: var(--white);
        font-size: 40px;
    }
    label, input {
        display: block;
        margin: 0 auto;
    }
    input[type=range] {
        margin: 10px auto;
    }
    input[type=number]:not(:last-of-type) {
        margin-bottom: 10vh;
    }
</style>