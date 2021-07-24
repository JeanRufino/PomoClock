<template>
    <!-- <button @click="toggleOptions"> -->
        <span class="material-icons-outlined menuM" @click="toggleOptions">
            settings
        </span>
        <span class="menu" @click="toggleOptions">Opções</span>
    <!-- </button> -->
    <div class="options" :class="{ active: isActive }">
        <div class="optionsHeader">
            <img src="../assets/cutTomato.png" alt="Um tomate cortado ao meio">
            <span class="menuTitle">Opções</span>
            <!-- <button @click="toggleOptions"> -->
                <span class="material-icons-outlined menuM"  @click="toggleOptions">
                    clear
                </span>
                <span class="menu" @click="toggleOptions">Fechar</span>
            <!-- </button> -->
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
        }
    }
}
</script>

<style scoped>
    /* ---- HEADER STARTS ----  */
    .options {
        position: fixed;
        display: flex;
        flex-direction: column;
        right: 0;
        top: 0;
        width: 100%;
        height: 100%;
        background: var(--red);
        overflow-y: auto;
        box-shadow: -10px 0px 0px 0px var(--dark-red);
        -webkit-transition: 300ms ease-out;
        -moz-transition: 300ms ease-out;
        -o-transition: 300ms ease-out;
        transition: 300ms ease-out;
        transform: translateX(110%);
        -o-transform: translateX(110%);
        -webkit-transform: translateX(110%);
        z-index: 2;
    }
    .options.active {
        transform: translateX(0);
        -o-transform: translateX(0);
        -webkit-transform: translateX(0);
    }
    .optionsHeader {
        display: grid;
        align-items: center;
        justify-content: center;
        grid-template-columns: 30% 40% 30%;
        padding: 30px 0 30px 0;
        background: var(--dark-red);
    }
    .optionsHeader img {
        margin-right: 0;
    }
    span.menu, span.menuTitle {
        font-size: 20px;
        font-weight: normal;
    }
    span.menu {
        display: none;
        -webkit-transition: 300ms ease-out;
        -moz-transition: 300ms ease-out;
        -o-transition: 300ms ease-out;
        transition: 300ms ease-out;
    }
    span.menu:hover { 
        letter-spacing: 5px;
    }
    span.menuM {
        color: var(--white);
        font-size: 24px;
        display: block;
    }
    span.menu, span.menuM { 
        margin-right: auto;
        cursor: pointer;
    }
    /* ---- HEADER ENDS ----  */
    /* ---- OPTIONS START ----  */
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
        color: var(--white);
        font-size: 20px;
        padding: 10px 14px;
        margin: 0 auto;
        border-radius: 4px;
        border: none;
        cursor: pointer;
    }
    input[type=range] {
        -webkit-appearance: none;
        width: 50vw;
        max-width: 320px;
        margin: 30px auto;
        background-color: transparent;
    }
    /* ---- OPTIONS END ----  */
    /* ---- RANGE INPUT RESETS BEGIN ---- */
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
    /* ---- RANGE INPUT RESETS ENDS ---- */
    /* ---- RANGE INPUT THUMB STYLING BEGINS ---- */
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
    /* ----RANGE INPUT THUMB STYLING ENDS ----*/
    /* ---- RANGE INPUT TRACK STYLING BEGINS ---- */
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
    /* ---- RANGE INPUT TRACK STYLING ENDS ---- */
    /* ---- NUMBER INPUT STYLING BEGINS ---- */
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
        max-width: 100px;           /* Firefox workaround, since it accepts no auto-width styles */
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
    /* ---- NUMBER INPUT STYLING ENDS ---- */
    /* ---- CONFIRMATION MODAL BEGINS ---- */
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
        -webkit-transition: 300ms ease-out;
        -moz-transition: 300ms ease-out;
        -o-transition: 300ms ease-out;
        transition: 300ms ease-out;
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
    /* ---- CONFIRMATION MODAL ENDS ---- */
    /* ---- MEDIA QUERIES BEGIN ---- */
    @media screen and (min-width: 500px) {
        span.menu, span.menuTitle {
            font-size: 24px;
            display: block;
        }
        span.menuM {
            font-size: 24px;
            display: none;
        }
        .cycleOptionsWrapper {
            display: flex;
            justify-content: center;
        }
        .cycleOptions {
            width: 50%;
        }
        label, input, .optionsHeader > span {
            font-size: 24px;
        }
        .buttonWrapper .resetButton {
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
        .cycleOptions {
            width: 33.3333%;
        }
    }
</style>