<script setup>
    import { Line } from 'vue-chartjs'
    import { Chart as ChartJS, CategoryScale, LinearScale, PointElement, LineElement, Title, Tooltip, Legend } from 'chart.js'
</script>

<template>
    <div id="chartcontainer">
        <Line :data="chartdata" :options="options"/>
    </div>
</template>

<script>
const genlabels = (() => {
    const start = 6
})()

export default {
    name: 'Chart',
    props: [ "dataarray" ],
    data() {
        return {
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    x: {
                        grid: {
                            color: "#355251"
                        },
                        ticks: {
                            maxTicksLimit: 24
                        }
                    },
                    y: {
                        grid: {
                            color: "#355251"
                        }
                    }
                }
            }
        }
    },
    computed: {
        chartdata() {
            return {
                labels: ['6:00AM', '6:00AM', '6:00AM', '6:00AM', '7:00AM','7:00AM','7:00AM','7:00AM', '8:00AM','8:00AM','8:00AM','8:00AM', '9:00AM','9:00AM','9:00AM','9:00AM', '10:00AM','10:00AM','10:00AM','10:00AM', '11:00AM','11:00AM','11:00AM','11:00AM', '12:00PM', '12:00PM', '12:00PM', '12:00PM', '1:00PM', '1:00PM', '1:00PM', '1:00PM', '2:00PM', '2:00PM','2:00PM','2:00PM', '3:00PM', '3:00PM','3:00PM','3:00PM','4:00PM' , '4:00PM' ,'4:00PM' ,'4:00PM' ,'5:00PM','5:00PM','5:00PM','5:00PM', '6:00PM', '6:00PM', '6:00PM', '6:00PM', '7:00PM', '7:00PM', '7:00PM', '7:00PM', '8:00PM', '8:00PM', '8:00PM', '8:00PM', '9:00PM',  '9:00PM', '9:00PM', '9:00PM','10:00PM', '10:00PM','10:00PM','10:00PM','11:00PM', '11:00PM','11:00PM','11:00PM', '12:00AM', '12:00AM','12:00AM','12:00AM', '1:00AM', '1:00AM','1:00AM','1:00AM','2:00AM', '2:00AM', '2:00AM', '2:00AM', '3:00AM','3:00AM','3:00AM','3:00AM', '4:00AM', '4:00AM', '4:00AM', '4:00AM', '5:00AM', '5:00AM', '5:00AM', '5:00AM'],
                datasets: [
                    {
                    label: 'Data One',
                    backgroundColor: '#73fffa',
                    borderColor: '#73fffa',
                    //data: [40, 39, 10, 40, 39, 80, 40, 40, 39, 10, 40, 39, 80, 40, 40, 39, 10, 40, 39, 80, 40, 39, 80, 40,40, 39, 10, 40, 39, 80, 40, 40, 39, 10, 40, 39, 80, 40, 40, 39, 10, 40, 39, 80, 40, 39, 80, 40,40, 39, 10, 40, 39, 80, 40, 40, 39, 10, 40, 39, 80, 40, 40, 39, 10, 40, 39, 80, 40, 39, 80, 40,40, 39, 10, 40, 39, 80, 40, 40, 39, 10, 40, 39, 80, 40, 40, 39, 10, 40, 39, 80, 40, 39, 80, 40],
                    data: this.dataarray,
                    cubicInterpolationMode: 'default'
                    }
                ]
            }
        }
    }
}

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
)
</script>

<style>
#chartcontainer {
    height: 500px;
}
</style>