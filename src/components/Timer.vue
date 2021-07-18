<template>
    <div class="timer">
        <div class="timer-value">
            {{timeOutput}}
        </div>
    </div>
    <div class="buttons">
        <button @click="click">
            {{buttonText}}
        </button>
    </div>
</template>

<script>
export default {
    name: "Timer",
    props: {},
    data() {
        const cycleDuration = 25;
        // const brakeDuration = 5;
        // const longBreakDuration = 30;

        return {
            cycleDuration,
            timeInSeconds: cycleDuration * 60,
            interval: null,
            cycle: 1,
            set: 1,
            buttonText: 'Iniciar',
        };
    },
    methods: {
        click() {
            if(this.buttonText === 'Pausar') {
                this.buttonSwap();
                return clearInterval(this.interval);
            }

            this.interval = setInterval(() => { 
                if(this.timeInSeconds > 0) {
                    this.timeInSeconds -= 1;
                } else if(this.timeInSeconds <= 0) {
                    // temp
                clearInterval(this.interval);
                this.end();
                }
            },1000)

            this.buttonSwap();
        },
        end() {
            if(this.cycle < 4) {
                this.cycle += 1;
                // this.rest(brakeDuration);
            } else {
                this.cycle = 1;
                this.set += 1;
                // this.rest(longBrakeDuration);
            }
            this.timeInSeconds = this.cycleDuration * 60;
        },
        rest() {
            return;
        },
        buttonSwap() {
            if(this.buttonText === 'Iniciar' || this.buttonText === 'Continuar') {
                this.buttonText = 'Pausar';
            } else {
                this.buttonText = 'Continuar';
            }
        }
    },
    computed: {
        timeOutput() {
            let minutes = parseInt(this.timeInSeconds / 60);
            let seconds = this.timeInSeconds % 60;
            let minutesS = ('0' + minutes).slice(-2);
            let secondsS= ('0' + seconds).slice(-2);
            return `${minutesS}:${secondsS}`
        }
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.timer {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50vh;
    width: 50vh;
    margin: 0 auto;
    border: 4px solid var(--text-black);
    border-radius: 50%;
}
.timer .timer-value {
    font-size: 50px;
}
button {
    margin-top: 50px;
    padding: 6px 24px;
    border: 4px solid var(--text-black);
    color: var(--text-wshite);
    font-weight: normal;
    font-size: 20px;
    cursor: pointer;
    background-color: var(--dark-red);
    transition: 900ms;
}
button:active {
    opacity: 0.5;
}
</style>
