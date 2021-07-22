<template>
    <button @click="toggleOptions">
        <span class="material-icons-outlined">
            settings
        </span>
    </button>
    <div class="options" :class="{ active: isActive }">
        <div v-for="option in options" :key="option.string">
            <label :for="option.string">{{option.label}}</label>

            <input v-model="option.selector" :id="option.string" type="range" min="0.1" max="60" @input="durationValidation($event, option.string, option.selector)">

            <input v-model.number="option.selector" :id="option.string" type="number" min="0.1" max="60" @input="durationValidation($event, option.string, option.selector)">
        </div>
    </div>
</template>

<script>
export default {
    name: 'TimerOptions',
    props: {
        cycleDuration: {
            type: Number,
            required: true,
        },     
        breakDuration: {
            type: Number,
            required: true,
        },     
        longBreakDuration: {
            type: Number,
            required: true,
        }, 
    },
    data() {
        return {
            isActive: true,
            cycleDurationCopy: this.cycleDuration,
            breakDurationCopy: this.breakDuration,
            longBreakDurationCopy: this.longBreakDuration,
            options: [
                { selector: this.cycleDuration, string: 'cycleDuration', label: 'Duração do ciclo' },
                { selector: this.breakDuration, string: 'breakDuration', label: 'Duração do descanço' },
                { selector: this.longBreakDuration, string: 'longBreakDuration', label: 'Duração do descanço longo' }
            ]
        }
    },
    methods: {
        toggleOptions() {
            this.isActive ? this.isActive = false : this.isActive = true;  
        },
        durationValidation(event, string, selector) {
            if(event.target.value > 60) {
                event.target.value = 60;
            } else if (event.target.value <= 0.1 || event.target.value == '' || event.target.value == null) {
                event.target.value = 0.1;
            }
            selector = event.target.value;
            this.$emit(`update:${string}`, selector );
            console.log(`Local: ${selector}, String: ${string}, Root: ${this.cycleDuration}, ${this.breakDuration}, ${this.longBreakDuration}`)
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