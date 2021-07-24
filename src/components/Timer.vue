<template>
    <div class="timer">
        <div class="timer-value">
            {{timeOutput}}
        </div>
    </div>
    <div class="cycleInfo">
        <div class="set">
            Série <br> <span>{{set}}</span>/{{maxSets}}
        </div>
        <button @click="click">
            <span class="material-icons-outlined">
                {{buttonText}}
            </span>
        </button>
        <div class="cycle">
            Ciclo <br> <span>{{cycle}}</span>/{{maxCycles}}
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
        maxSets: { type: Number, required: true, },
        maxCycles: { type: Number, required: true, },
        cycle: { type: Number, required: true, },
        set: { type: Number, required: true, },
    },
    data() {
        return {
            timeInSeconds: this.cycleDuration * 60,   
            elapsedTime: 0,
            
            interval: null,
            buttonText: 'play_circle_filled',
            audio: new Audio(bells),
        }
    },
    watch: {
        isCycle(newVal) {
            if(newVal) {
                this.timeInSeconds = this.cycleDuration * 60
                this.elapsedTime = 0;    
            }
        },
        isBreak(newVal) {
            if(newVal) {
                this.timeInSeconds = this.breakDuration * 60
                this.elapsedTime = 0;    
            }
        },
        isLongBreak(newVal) {
            if(newVal) {
                this.timeInSeconds = this.longBreakDuration * 60
                this.elapsedTime = 0;    
            }
        },
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
            if(this.isCycle) {
                this.$emit('update:isCycle', false);
                if(this.cycle < 4) {
                    this.$emit('update:cycle', this.cycle + 1);
                    this.$emit('update:isBreak', true);
                } else if(this.cycle == 4) {
                    this.$emit('update:cycle', 1)
                    this.$emit('update:set', this.set + 1);
                    this.$emit('update:isLongBreak', true);
                }
                return this.newInterval();
            } else {
                this.$emit('update:isCycle', true);
                this.$emit('update:isBreak', false);
                this.$emit('update:isLongBreak', false);
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
    min-height: 50vw;
    min-width: 50vw;
    margin: 0 auto;
    border: 4px solid var(--white);
    border-radius: 50%;
}
.timer .timer-value {
    font-size: 44px;
}
.cycleInfo {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 30px auto;
    width: 100%;
}
button {
    display: flex;
    align-items: center;
    background-color: transparent;
    border: none;
    color: var(--white);
    font-weight: normal;
    cursor: pointer;
    transition: 300ms;
    width: 34%;
}
button:active {
    opacity: 0.5;
}
button > span {
    font-size: 70px;
    margin: 0 auto;
}
.set, .cycle {
    width: 33%;
    font-size: 20px;
}
.set > span, .cycle > span {
    font-size: 40px;
}
/* ---- MEDIA-QUERIES ---- */
@media screen and (min-width: 500px) {
    .timer {
        min-height: unset;
        min-width: unset;
        min-height: 320px;
        min-width: 320px;
        border: 4px solid var(--white);
    }
    .timer .timer-value {
        font-size: 56px;
    }
    .set, .cycle {
        font-size: 28px;
    }
    .set > span, .cycle > span {
        font-size: 48px;
    }
}
@media screen and (min-width: 780px) {
    .cycleInfo {
        width: 60%;
    }
}
</style>
