<template>
    <button @click="toggleOptions">
        <span class="material-icons-outlined">
            settings
        </span>
    </button>
    <div class="options" :class="{ active: isActive }">
        <div class="optionsHeader">
            <span>Opções</span>
            <button @click="toggleOptions">
                <span class="material-icons-outlined">
                    clear
                </span>
            </button>
        </div>
        <div v-for="(option, index) in options" :key="option.string">
            <label :for="option.string">{{option.label}}</label>

            <input v-model.number="option.selector" :id="option.string" type="range" min="1" max="60" @input="durationValidation($event, option.string, option.selector, index)">

            <input v-model.number="option.selector" :id="option.string" type="number" min="1" max="60" @input="durationValidation($event, option.string, option.selector, index)" @click="select">
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
        isActive: {
            type: Boolean,
            required: true,
        },
    },
    data() {
        return {
            cycleDurationCopy: this.cycleDuration,
            breakDurationCopy: this.breakDuration,
            longBreakDurationCopy: this.longBreakDuration,
            options: [
                { selector: parseFloat(this.cycleDuration), string: 'cycleDuration', label: 'Duração do ciclo' },
                { selector: parseFloat(this.breakDuration), string: 'breakDuration', label: 'Duração do descanço' },
                { selector: parseFloat(this.longBreakDuration), string: 'longBreakDuration', label: 'Duração do descanço longo' }
            ]
        }
    },
    watch: {
        cycleDuration(newVal) {
            if(this.isCycle) this.timeInSeconds = (newVal * 60) - this.elapsedTime; 
        },
        breakDuration(newVal) {
            if(this.isBreak) this.timeInSeconds = (newVal * 60) - this.elapsedTime;
        },
        longBreakDuration(newVal) {
            if(this.isLongBreak) this.timeInSeconds = (newVal * 60) - this.elapsedTime;
        },
    },
    methods: {
        toggleOptions() {
            this.isActive ? this.$emit('update:isActive', false) : this.$emit('update:isActive', true)
        },
        durationValidation(event, string, selector, index) {
            if(event.target.value > 60 || event.target.value.length > 2) {
                event.target.value = 60; 
                this.options[index].selector = 60;
                console.log(this.options[index].selector)
            } else if (event.target.value <= 1 || event.target.valuelength < 0) {
                event.target.value = 1; 
                this.options[index].selector = 1;
            }
            selector = parseFloat(event.target.value);
            this.$emit(`update:${string}`, selector );
        },
        select(e) {
            e.target.select()
        }
    }
}
</script>

<style scoped>
    .options {
        position: fixed;
        display: flex;
        flex-direction: column;
        right: 0;
        top: 0;
        width: 100%;
        height: 100%;
        background-color: var(--red);
        transition: 300ms ease;
        z-index: 2;
        box-shadow: -10px 0px 0px 0px var(--dark-red);
        -webkit-transform: translateX(110%);
        -o-transform: translateX(110%);
        transform: translateX(110%);
        overflow-y: auto;
    }
    .options.active {
        -webkit-transform: translateX(0);
        -o-transform: translateX(0);
        transform: translateX(0);
    }
    .optionsHeader {
        display: grid;
        align-items: center;
        grid-template-columns: 1fr calc(32vw + 44px) 1fr;
        grid-template-areas: "a b c";
        margin: 30px 0 20px 0;
        height: 44px;
        background: var(--dark-red);
    }
    .optionsHeader > button {
        grid-area: c;
    }
    .optionsHeader > span {
        font-size: 20px;
        grid-area: b;
    }
    button { 
        display: flex;
        align-items: center;
        border: none;
        background-color: transparent;
        width: max-content;
        align-items: center;
        margin-right: auto;
        cursor: pointer;
        transition: 300ms;
    }
    button:hover { 
        opacity: 0.6;
    }
    button span {
        color: var(--white);
        font-size: 26px;
    }
    label, input {
        font-size: 20px;
        display: block;
        margin: 0 auto;
    }
    input[type=range] {
        -webkit-appearance: none;
        width: 50vw;
        max-width: 320px;
        margin: 24px auto;
        background-color: transparent;
    }
    /* RANGE INPUT RESETS */
    input[type=range]::-webkit-slider-thumb {
        -webkit-appearance: none;
    }
    input[type=range]:focus {
        outline: none;
    }
    input[type=range]::-ms-track {
        width: 100%;
        cursor: pointer;
        background: transparent; 
        border-color: transparent;
        color: transparent;
    }
    /* RANGE INPUT THUMB STYLING BEGINS */
    input[type=range]::-webkit-slider-thumb {
        -webkit-appearance: none;
        margin-top: -10px; 
        border: 2px solid var(--dark-red);
        height: 24px;
        width: 24px;
        border-radius: 50%;
        background: var(--white);
        cursor: pointer;
    }
    input[type=range]::-moz-range-thumb {
        border: 2px solid var(--dark-red);
        height: 24px;
        width: 24px;
        border-radius: 50%;
        background: var(--white);
        cursor: pointer;
    }
    input[type=range]::-ms-thumb{
        border: 2px solid var(--dark-red);
        height: 24px;
        width: 24px;
        border-radius: 50%;
        background: var(--white);
        cursor: pointer;
    }
    /* RANGE INPUT THUMB STYLING ENDS */
    /* RANGE INPUT TRACK STYLING BEGINS */
    input[type=range]::-webkit-slider-runnable-track {
        width: 100%;
        height: 8.4px;
        cursor: pointer;
        background: var(--white);
        border: 2px solid var(--dark-red);
    }
    input[type=range]:focus::-webkit-slider-runnable-track {
        background: var(--white);
    }
    input[type=range]::-moz-range-track {
        width: 100%;
        height: 8.4px;
        cursor: pointer;
        background: var(--white);
        border: 2px solid var(--dark-red);
    }
    input[type=range]::-ms-track {
        width: 100%;
        height: 8.4px;
        cursor: pointer;
        background: transparent;
        border-color: transparent;
        border-width: 16px 0;
        color: transparent;
    }
    /* RANGE INPUT TRACK STYLING ENDS */
    input[type=number] {
        text-align: center;
        font-family: 'Poppins', sans-serif;
        color: var(--white);
        font-weight: 500;
        background-color: var(--dark-red);
        border-radius: 4px;
        padding: 4px;
        border: none;
        margin-bottom: 30px;
        width: 33%;
        -moz-appearance: textfield;
    }
    input[type=number]:focus {
        outline: none;
    }
    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }
    /* MEDIA QUERIES */
    @media screen and (min-width: 500px) {
        button span {
            font-size: 30px;
        }
        label, input, .optionsHeader > span {
            font-size: 24px;
        }
    }
</style>