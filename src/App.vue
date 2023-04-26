<script setup>
    import Chart from './components/Chart.vue'
    import EventList from './components/EventList.vue'
</script>

<template>
    <table id="container">
        <tr>
            <td>
                <EventList @dataupdate="onChange"></EventList>
            </td>
            <td>
                <Chart :dataarray="dataarray.intervals" :highpointarray="dataarray.highpoints"></Chart>
            </td>
        </tr>
    </table>
</template>

<script>
export default {
    components: {
        Chart,
        EventList
    },
    data() {
        return {
            dataarray: []
        }
    },
    methods: {
        onChange(array) {
            console.log(array)
            this.dataarray = JSON.parse(JSON.stringify(array))
        }
    }
}
</script>

<style>
#container {
    height: 650px;
    width: 1200px;
    border: 1px solid red;
}
</style>
