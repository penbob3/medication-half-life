<template>
    <div id="controlcontainer">
        <div id="medcontrols">
            Half-Life (Hours): &nbsp;<input type="number" min="0.5" max="12" style="width: 60px;" v-model="halflifein">&nbsp;&nbsp;&nbsp;&nbsp;
            Peak Plasma (Hours): &nbsp;<input type="number" min="0.25" max="12" style="width: 60px;" v-model="peakplasmain">
        </div>
        <div id="eventcontainer">
            <div class="eventlistitem" v-for="(eventline, idx) in events" :key="eventline.id">
                <select style="width: 70px;" v-model="eventline.hour">
                    <option v-for="index in 11" :value="index" :key="index">{{ index }}</option>
                    <option :value="0">12</option>
                </select>
                &nbsp; : &nbsp;
                <select style="width: 70px;" v-model="eventline.min">
                    <option v-for="index in 4" :value="index" :key="index"> {{ index == 1 ? '00' : ((index - 1) * 15) }}</option>
                </select>
                <select style="width: 70px;" v-model="eventline.ampm">
                    <option value="0">AM</option>
                    <option value="1">PM</option>
                </select>
                <span style="user-select: none;">&nbsp; Dose: &nbsp;</span>
                <input type="number" min="0" max="5000" style="width: 60px;" v-model="eventline.dose" >
                <div class="removebtn" @click="removeEvent(idx)"><div class="btnwrap"><div class="x f"></div><div class="x r"></div></div></div>
            </div>
            <div id="addeventbtn" @click="addEvent()"><span id="addeventtext">Add Dose...</span></div>
        </div>
    </div>
</template>

<script>
export default {
    name: "EventList",
    data() {
        return {
            events: [
                {
                    id: 1519211811670,
                    hour: 6,
                    min: 2,
                    ampm: 0,
                    dose: 5
                }
            ],
            halflifein: "5",
            peakplasmain: "1.5"
        }
    },
    mounted() {
        this.genGraphArray()
    },
    methods: {
        roundNearest(value, nearest) { return Math.round(value / nearest) * nearest },
        addEvent() {
            this.events.push({
                id: Date.now(),
                hour: 1,
                min: 2,
                ampm: 0,
                dose: 5
            },)
            this.genGraphArray()
        },
        removeEvent(idx) {
            //this.events = this.events.filter(item => item !== value)
            this.events.splice(idx, 1)
            this.genGraphArray()
        },
        genGraphArray() {

            console.log(this.events)
            let eventlist = []

            //Event Converter
            this.events.forEach((event, index) => {
                let houradj
                if (event.hour >= 6) {
                    houradj = event.hour - 6
                } else {
                    houradj = 24 - event.hour
                }
                let hoursegcount = houradj * 4
                let minsegcount = event.min
                let totalsegcount = hoursegcount + minsegcount

                let evlengthmin = (this.peakplasmain * 60)
                let evlengthsegcount = (this.roundNearest(evlengthmin, 15)) / 15

                let convertedEvent = {
                    startseg: totalsegcount,
                    length: evlengthsegcount,
                    change: event.dose
                }

                eventlist.push(convertedEvent)
            })

            //console.log(eventlist)

            let event1 = {
                startseg: 5, //Start of the event
                length: 2, //Lenght of the event (in 15min segments)
                change: 5 //Increase/Decrease amount
            }

            let event2 = {
                startseg: 20,
                length: 3,
                change: 5
            }

            //eventlist.push(event1)
            //eventlist.push(event2)

            const intervalsegcount = 96 //How many 15min intervals to output
            let intervalarray = [] //Array of the value of each interval
            let highpointarray = [] //Array of which intervals should have points on them
            let currentamount = 0 //The current amount of drug present
            let halflife = this.halflifein //Half life in hours
            let decayrate = 0.693 / (halflife * 4) //Amount per interval that the drug is reduced by
            console.log(decayrate)

            for (let i = 0; i < intervalsegcount; i++) {
                let isHighPoint = 0
                eventlist.forEach((event, idx) => {
                    if (i >= event.startseg && i < (event.startseg + event.length)) {
                        currentamount += (event.change / event.length)
                        isHighPoint = 4
                    }
                })
                currentamount = currentamount - (currentamount * decayrate)
                intervalarray.push(currentamount)
                highpointarray.push(isHighPoint)
            }

            console.log('help')
            this.$emit('dataupdate', {intervals: intervalarray, highpoints: highpointarray})
        }
    },
    watch: {
        events: {
            handler: function() {
                console.log('AAAAA')
                this.genGraphArray()
            },
            deep: true
        },
        halflifein() {
            this.genGraphArray()
        },
        peakplasmain() {
            this.genGraphArray()
        }
    }
}
</script>

<style>

#controlcontainer {
    height: 295px;
}

#medcontrols {
    height: 45px;
    width: 430px;
    display: flex;
    justify-content: center;
    align-items: center;
}

#eventcontainer {
    width: 430px;
    height: 250px;
    border: 1px solid red;
    overflow-x: hidden;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    flex-wrap: nowrap;
    justify-content: flex-start;
    align-items: center;
    row-gap: 15px;
    padding: 15px 0 15px 0;
}

.eventlistitem {
    width: 90%;
    height: 40px;
    min-height: 40px;
    border-radius: 10px;
    border: 1px solid red;
    background-color: #1b2831;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 0 4px 0 4px;
}

.removebtn {
    width: 25px;
    height: 25px;
    margin: 0 0 0 5px;
}

.removebtn:hover .x {
    background-color: rgb(222, 109, 109);
}

.x {
    position: absolute;
    height: 4px;
    width: 23px;
    margin: 11px 0 0 1px;
    background-color: rgb(207, 213, 218);
    transition: 0.15s ease-in-out;
}

.f {
    transform: rotate(45deg);
}

.r {
    transform: rotate(-45deg);
}

#addeventbtn {
    width: 22%;
    height: 40px;
    min-height: 40px;
    border-radius: 10px;
    border: 1px solid red;
    background-color: #1b2831;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0 4px 0 4px;
    user-select: none;
}

#addeventtext {
    transition: 0.15s ease-in-out;
}

#addeventbtn:hover #addeventtext {
    color: #0ab13f;
}
</style>

<style lang="scss" scoped>
$glass: rgba(255, 255, 255, 0.2);
$glass-icon: rgba(255, 255, 255, 0.3);
$option: #320a28;

select {
  /* Reset */
  appearance: none;
  border: 0;
  outline: 0;
  font: inherit;
  /* Personalize */
  width: 10em;
  height: 2em;
  padding: 0 2em 0 0.8em;
  background: url(https://upload.wikimedia.org/wikipedia/commons/9/9d/Caret_down_font_awesome_whitevariation.svg)
      no-repeat right 0.34em center / 1.4em,
    linear-gradient(to left, $glass-icon 2em, $glass 2em);
  color: rgb(255, 255, 255);
  border-radius: 0.25em;
  box-shadow: 0 0 1em 0 rgba(27, 27, 27, 0.2);
  cursor: pointer;
  /* <option> colors */
  option {
    color: inherit;
    background-color: $option;
  }
  /* Remove focus outline */
  &:focus {
    outline: none;
  }
  /* Remove IE arrow */
  &::-ms-expand {
    display: none;
  }
}

</style>