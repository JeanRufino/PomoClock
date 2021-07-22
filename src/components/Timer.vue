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
        cycleDuration: { type: Number, required: true, },     
        breakDuration: { type: Number, required: true, },     
        longBreakDuration: { type: Number, required: true, },
        isCycle: { type: Boolean, required: true, },
        isBreak: { type: Boolean, required: true, },
        isLongBreak: { type: Boolean, required: true, },
    },
    data() {
        return {
            isCycleCopy: this.isCycle,
            isBreakCopy: this.isBreak,
            isLongBreakCopy: this.isLongBreak,
            timeInSeconds: this.cycleDuration * 60,
            elapsedTime: 0,
            cycle: 1,
            set: 1,
            interval: null,
            buttonText: 'play_circle_filled',
            audio: new Audio(bells),
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
        timeInSeconds(newVal) {
            this.timeInSeconds = newVal;
        }
    },
    methods: {
        click() {
            if(this.buttonText === 'pause_circle_filled') {
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
                    this.elapsedTime += 1;
                } else if(this.timeInSeconds <= 0) {
                    clearInterval(this.interval);
                    this.end();
                }
            }, 1000)
        },
        end() {
            this.playBells();
            if(this.isCycleCopy) {
                this.isCycleCopy = false;
                if(this.cycle < 4) {
                    this.cycle += 1;
                    this.isBreakCopy = true;
                    this.$emit('update:isCycle', this.isCycleCopy );
                    this.$emit('update:isBreak', this.isBreakCopy );
                    this.timeInSeconds = this.breakDuration * 60;
                    this.elapsedTime = 0;
                } else if(this.cycle == 4) {
                    this.cycle = 1;
                    this.set += 1;
                    this.isLongBreakCopy = true;
                    this.$emit('update:isCycle', this.isCycleCopy );
                    this.$emit('update:isLongBreak', this.isLongBreakCopy );
                    this.timeInSeconds = this.longBreakDuration * 60;
                    this.elapsedTime = 0;
                }
                return this.newInterval();
            } else {
                this.isCycleCopy = true;
                this.isBreakCopy = false;
                this.isLongBreakCopy = false;
                this.$emit('update:isCycle', this.isCycleCopy );
                this.$emit('update:isBreak', this.isBreakCopy );
                this.$emit('update:isLongBreak', this.isLongBreakCopy );
                this.timeInSeconds = this.cycleDuration * 60;
                this.elapsedTime = 0;
                return this.newInterval(); 
            }
        },
        buttonSwap() {
            if(this.buttonText === 'play_circle_filled') {
                this.buttonText = 'pause_circle_filled';
            } else {
                this.buttonText = 'play_circle_filled';
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
}
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
}
.timer .timer-value {
    font-size: 50px;
}
.cycleInfo {
    display: flex;
    justify-content: center;
    align-items: center;
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
