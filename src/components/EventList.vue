<template>
    <div id="eventcontainer">
        <div class="eventlistitem">Wow!</div>
        <div class="eventlistitem">Wow!</div>
        <div class="eventlistitem">Wow!</div>
    </div>
</template>

<script>
export default {
    name: "EventList",
    mounted: function () {
        let eventlist = []

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

        eventlist.push(event1)
        eventlist.push(event2)

        const intervalsegcount = 96 //How many 15min intervals to output
        let intervalarray = [] //Array of the value of each interval
        let highpointarray = [] //Array of which intervals should have points on them
        let currentamount = 0 //The current amount of drug present
        let halflife = 10 //Half life in hours
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
}
</script>

<style>
#eventcontainer {
    width: 400px;
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
    border-radius: 10px;
    border: 1px solid red;
    background-color: #1b2831;
}
</style>