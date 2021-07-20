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
    <div class="cycleInfo">
        <div class="set">
            Série: <br> {{set}}
        </div>
        <div class="cycle">
            Ciclo: <br> {{cycle}}/4
        </div>
    </div>
</template>

<script>
import bells from "../assets/bells.wav";

export default {
    name: "Timer",
    props: {
        cycleType: {
            type: Object,
            required: true
        }
    },
    emits: ['update:cycleType'],
    data() {
        const cycleDuration = 25;
        const breakDuration = 5;
        const longBreakDuration = 30;

        return {
            cycleDuration,
            breakDuration,
            longBreakDuration,
            isCycle: this.cycleType.isCycle,
            isBreak: this.cycleType.isBreak,
            isLongBreak: this.cycleType.isLongBreak,
            timeInSeconds: cycleDuration * 60,
            interval: null,
            cycle: 1,
            set: 1,
            buttonText: 'Iniciar',
            audio: new Audio(bells),
        };
    },
    methods: {
        click() {
            if(this.buttonText === 'Pausar') {
                this.buttonSwap();
                return clearInterval(this.interval);
            }
            this.newInterval();
            this.buttonSwap();
        },
        newInterval() {
            this.interval = setInterval(() => { 
                 if(this.timeInSeconds > 0) {
                    this.timeInSeconds -= 1;
                } else if(this.timeInSeconds <= 0) {
                    clearInterval(this.interval);
                    this.end();
                }
            }, 1000)
        },
        end() {
            this.playBells();
            if(this.isCycle) {
                this.isCycle = false;
                if(this.cycle < 4) {
                    this.cycle += 1;
                    this.isBreak = true;
                    this.isLongBreak = false;
                    this.$emit('update:cycleType', { 
                        isCycle: this.isCycle,
                        isBreak: this.isBreak,
                        isLongBreak: this.isLongBreak,
                    });
                    this.timeInSeconds = this.breakDuration * 60;
                } else if(this.cycle == 4) {
                    this.cycle = 1;
                    this.set += 1;
                    this.isBreak = false;
                    this.isLongBreak = true;
                    this.$emit('update:cycleType', { 
                        isCycle: this.isCycle,
                        isBreak: this.isBreak,
                        isLongBreak: this.isLongBreak,
                    });
                    this.timeInSeconds = this.longBreakDuration * 60;
                }
                return this.newInterval();
            } else {
                this.isCycle = true;
                this.isBreak = false;
                this.isLongBreak = false;
                this.$emit('update:cycleType', { 
                        isCycle: this.isCycle,
                        isBreak: this.isBreak,
                        isLongBreak: this.isLongBreak,
                    });
                this.timeInSeconds = this.cycleDuration * 60;
                return this.newInterval(); 
            }
        },
        buttonSwap() {
            if(this.buttonText === 'Iniciar' || this.buttonText === 'Continuar') {
                this.buttonText = 'Pausar';
            } else {
                this.buttonText = 'Continuar';
            }
        },
        playBells() {
            this.audio.play();
        }
    },
    computed: {
        timeOutput() {
            let minutes = parseInt(this.timeInSeconds / 60);
            let seconds = parseInt(this.timeInSeconds % 60);
            let minutesParsed = ('0' + minutes).slice(-2);
            let secondsParsed= ('0' + seconds).slice(-2);
            return `${minutesParsed}:${secondsParsed}`
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
    background-color: var(--white);
    transition: 900ms;
}
button:active {
    opacity: 0.5;
}
.cycleInfo, 
.set, .cycle {
    display: flex;
    justify-content: center;
    align-items: center;
}
.cycleInfo {
    width: 100%;
    gap: 10px;
} 
.set, .cycle {
    background-color: var(--white);
    border: 4px solid var(--text-black);
    margin-top: 50px;
    font-size: 20px;
    width: 20%;
    padding: 6px 24px;
}
</style>
