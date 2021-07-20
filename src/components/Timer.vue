<template>
    <div class="timer">
        <div class="timer-value">
            {{timeOutput}}
        </div>
    </div>
    <div class="cycleInfo">
        <div class="set">
            Série <br> <span>{{set}}</span>/12
        </div>
        <div class="button">
            <button @click="click">
                <span class="material-icons-outlined">
                    {{buttonText}}
                </span>
            </button>
        </div>
        <div class="cycle">
            Ciclo <br> <span>{{cycle}}</span>/4
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
        const cycleDuration = 0.1;
        const breakDuration = 0.05;
        const longBreakDuration = 0.1;

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
            buttonText: 'play_circle',
            audio: new Audio(bells),
        };
    },
    methods: {
        click() {
            if(this.buttonText === 'pause_circle') {
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
                    this.$emit('update:cycleType', { 
                        isCycle: this.isCycle,
                        isBreak: this.isBreak,
                        // isLongBreak: this.isLongBreak,
                    });
                    this.timeInSeconds = this.breakDuration * 60;
                } else if(this.cycle == 4) {
                    this.cycle = 1;
                    this.set += 1;
                    this.isLongBreak = true;
                    this.$emit('update:cycleType', { 
                        isCycle: this.isCycle,
                        // isBreak: this.isBreak,
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
            if(this.buttonText === 'play_circle') {
                this.buttonText = 'pause_circle';
            } else {
                this.buttonText = 'play_circle';
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
    height: 50vw;
    width: 50vw;
    max-height: 320px;
    max-width: 320px;
    margin: 0 auto;
    border: 6px solid var(--white);
    border-radius: 50%;
    /* color: var(--text-black); */
}
.timer .timer-value {
    font-size: 50px;
}
.cycleInfo {
    display: flex;
    justify-content: center;
    align-items: center;
    /* color: var(--text-black); */
    margin-top: 30px;
    width: 100%;
}
button {
    background-color: transparent;
    border: none;
    color: var(--white);
    font-weight: normal;
    cursor: pointer;
    margin: 0 12vw;
    transition: 300ms;
}
.button span {
    font-size: 80px;
}
button:active {
    opacity: 0.5;
}
.set, .cycle {
    font-size: 24px;
}
.set > span, .cycle > span {
    font-size: 44px;
}
</style>
