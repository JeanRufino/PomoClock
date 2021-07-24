<template>
    <button @click="toggleOptions">
        <span class="material-icons-outlined menuM">
            settings
        </span>
        <span class="menu">Opções</span>
    </button>
    <div class="options" :class="{ active: isActive }">
        <div class="optionsHeader">
            <span>Opções</span>
            <button @click="toggleOptions">
                <span class="material-icons-outlined menuM">
                    clear
                </span>
                <span class="menu">Fechar</span>
            </button>
        </div>
        <div class="timeOptionsWrapper">
            <div v-for="(option, index) in options" :key="option.string" class="timeOptions">
                <label :for="option.string">{{option.label}}</label>

                <input v-model.number="option.selector" :id="option.string" type="number" min="1" max="60" @input="durationValidation($event, option.string, option.selector, index)" @click="select">

                <input v-model.number="option.selector" :id="option.string" type="range" min="1" max="60" @input="durationValidation($event, option.string, option.selector, index)">
            </div>
        </div>
        <div class="cycleOptionsWrapper">
            <div v-for="(option, index) in csOptions" :key="option.string" class="cycleOptions">
                <label :for="option.string">{{option.label}}</label>

                <input v-model.number="option.selector" :id="option.string" type="number" min="1" max="10" @input="csValidation($event, option.string, option.selector, index, option.max)" @click="select">
            </div>
        </div>
        <div class="buttonWrapper">
            <button @click="modal = true" class="resetButton">Resetar</button>
        </div>
    </div>
    <div :class="{ active: modal }" class="modalWrapper" @click="modal = false">
        <div class="modal" @click.stop="">
            <span>Resetar todos os dados para as configurações iniciais?</span>
            <div class="buttonWrapper">
                <button @click="reset(); modal = false">Confirmar</button>
                <button @click="modal = false">Cancelar</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TimerOptions',
    props: {
        cycleDuration: { type: Number, required: true, },     
        breakDuration: { type: Number, required: true, },     
        longBreakDuration: { type: Number, required: true, },
        isActive: { type: Boolean, required: true, },
        maxSets: { type: Number, required: true, },
        maxCycles: { type: Number, required: true, },
        isReset: { type: Boolean, required: true, },
    },
    data() {
        return {
            cycleDurationCopy: this.cycleDuration,
            breakDurationCopy: this.breakDuration,
            longBreakDurationCopy: this.longBreakDuration,
            options: [
                { selector: parseFloat(this.cycleDuration), string: 'cycleDuration', label: 'Duração do ciclo' },
                { selector: parseFloat(this.breakDuration), string: 'breakDuration', label: 'Duração da pausa' },
                { selector: parseFloat(this.longBreakDuration), string: 'longBreakDuration', label: 'Duração da pausa longa' }
            ],
            csOptions: [
                { selector: parseInt(this.maxSets), string: 'maxSets', label: 'Número de séries' },
                { selector: parseInt(this.maxCycles), string: 'maxCycles', label: 'Ciclos por série' },
            ],
            modal: false,
        }
    },
    watch: {
        cycleDuration(newVal) { this.options[0].selector = newVal; },
        breakDuration(newVal) { this.options[1].selector = newVal; },
        longBreakDuration(newVal) { this.options[2].selector = newVal; },
        maxSets(newVal) { this.csOptions[0].selector = newVal; },
        maxCycles(newVal) { this.csOptions[1].selector = newVal; },
    },
    methods: {
        toggleOptions() {
            this.isActive ? this.$emit('update:isActive', false) : this.$emit('update:isActive', true)
        },
        durationValidation(event, string, selector, index) {
            if(event.target.value > 60 || event.target.value.length > 2) {
                event.target.value = 60; 
                this.options[index].selector = 60;
            } else if(event.target.value < 1 || event.target.value.length < 1) {
                event.target.value = 1; 
                this.options[index].selector = 1;
            }
            selector = parseFloat(event.target.value);
            this.$emit(`update:${string}`, selector );
        },
        csValidation(event, string, selector, index) {
            if(event.target.value >= 10 || event.target.value.length > 2) {
                event.target.value = 10;
                this.csOptions[index].selector = 10;
            } else if(event.target.value < 1 || event.target.value.length < 1) {
                event.target.value = 1;
                this.csOptions[index].selector = 1;
            }
            selector = parseInt(event.target.value);
            this.$emit(`update:${string}`, selector);
        },
        select(e) {
            e.target.select();
        },
        reset() {
            this.$emit('update:isReset', true);
        },
        toggleConfirmation() {
            return;
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
        grid-template-rows: 44px;
        grid-template-areas: "a b c";
        padding: 30px 0 30px 0;
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
    button > .menu {
        display: none;
    }
    button > .menuM {
        display: block;
    }
    button span, .buttonWrapper button {
        color: var(--white);
        font-size: 26px;
    }
    label, input {
        width: max-content;
        font-size: 20px;
        display: block;
        margin: 30px auto 0;
    }
    .timeOptions, .cycleOptions {
        border-bottom: 2px solid var(--dark-red);
    }
    .buttonWrapper {
        width: 100%;
        margin: 30px auto;
    }
    .buttonWrapper button {
        background: var(--dark-red);
        padding: 10px 14px;
        margin: 0 auto;
        border-radius: 4px;
    }
    input[type=range] {
        -webkit-appearance: none;
        width: 50vw;
        max-width: 320px;
        margin: 30px auto;
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
        margin: 10px auto 30px;
        -moz-appearance: textfield;
        width: intrinsic;           /* Safari/WebKit uses a non-standard name */
        max-width: 25%;             /* Firefox workaround */
        width: -webkit-min-content; /* Chrome */
    }
    input[type=number]:focus {
        outline: none;
    }
    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }
    /* ---- CONFIRMATION MODAL ---- */
    .modalWrapper {
        position: fixed;
        padding: 40px;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background: var(--modal-black);
        display: none;
        opacity: 0;
        transition: 300ms;
        z-index: 3;
    }
    .modalWrapper.active {
        display: block;
        opacity: 1;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .modal {
        background: var(--white);
        color: var(--text-black);
        z-index: 3;
        width: 60vw;
        padding: 10px;
        border-radius: 4px;
    }
    .modal span, .modal button {
        font-size: 16px;
    }
    .modal .buttonWrapper button {
        padding: 6px;
    }
    .modal > .buttonWrapper {
        display: flex;
        margin: 20px auto 4px;
    }
    /* ---- MEDIA QUERIES ---- */
    @media screen and (min-width: 500px) {
        button span {
            font-size: 30px;
        }
        button > .menu {
            display: block;
            font-size: 24px;
        }
        button > .menuM {
            display: none;
        }
        .cycleOptionsWrapper {
            display: flex;
        }
        .cycleOptions {
            width: 50%;
        }
        label, input, .optionsHeader > span {
            font-size: 24px;
        }
        .modal {
            width: 40vw;
        }
        .modal .buttonWrapper button {
            padding: 10px;
        }
    }
    @media screen and (min-width: 1024px) {
        .timeOptionsWrapper {
            display: flex;
        }
        .timeOptions {
            width: 33.3333%;
        }
        input[type=range] {
            width: 50%;
        }
    }
</style>