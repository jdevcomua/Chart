<template>
    <div>
        <div id="chartdiv" ref="chartdiv">
        </div>
        <select class="selected" v-model="select" @change="changeData">
            <option>first</option>
            <option>second</option>
        </select>

    </div>
</template>

<script>

    import Vue from 'vue' ;
    import axios from 'axios';
    import VueAxios from 'vue-axios';

    Vue.use(VueAxios, axios);

    import * as am4core from "@amcharts/amcharts4/core";
    import * as am4charts from "@amcharts/amcharts4/charts";
    import am4themes_animated from "@amcharts/amcharts4/themes/animated";

    am4core.useTheme(am4themes_animated);


    export default {
        name: 'chart',
        data: () => ({
            chart: null,
            select: 'first',
            data:[],
        }),
        mounted() {
            this.getData();
        },
        methods:{
            draw(data){
                this.chart = am4core.create(this.$refs.chartdiv, am4charts.RadarChart);
                this.chart.data = data;

                /* Create axes */
                let categoryAxis = this.chart.xAxes.push(new am4charts.CategoryAxis());
                categoryAxis.dataFields.category = "label";


                let valueAxis = this.chart.yAxes.push(new am4charts.ValueAxis());
                valueAxis.renderer.axisFills.template.fill = this.chart.colors.getIndex(2);
                valueAxis.renderer.axisFills.template.fillOpacity = 0.05;
                valueAxis.renderer.gridType = 'polygons';
                valueAxis.renderer.labels.template.disabled = true;
                valueAxis.min = 10;
                valueAxis.max = 50;

                /* Create and configure series */
                let series = this.chart.series.push(new am4charts.RadarSeries());
                series.dataFields.valueY = "value";
                series.dataFields.categoryX = "label";
                series.strokeWidth = 3;
                series.tooltip.autoTextColor = false;
                series.tooltip.label.fill = am4core.color("black");
                series.fill = am4core.color("#ccc");

                let circleBullet = series.bullets.push(new am4charts.CircleBullet());
                circleBullet.circle.fill = am4core.color("#fff");
                circleBullet.circle.strokeWidth = 3;
                circleBullet.tooltipText = "[bold]{label}[/] \n {value} points[/]";


                this.chart.cursor = new am4charts.RadarCursor();
            },
            changeData(){
                this.chart.dispose();
                return this.getData();
            },
            getData(){
                // let url = './data.json';
                // if (this.select === 'second'){
                //     url = './data2.json'
                // }
                // this.axios.get(url).then(response => {
                //     console.log(response);
                //     this.data = response.data;
                //     this.draw(this.data);
                // }).catch(error => {
                //     console.log(error);
                // });
                this.data =  [
                    {"label": "JavaScript", "value": 30},
                    {"label": "HTML", "value": 35},
                    {"label": "Flinto", "value": 33},
                    {"label": "Vue.js", "value": 32},
                    {"label": "Sketch", "value": 30},
                    {"label": "Priciple", "value": 30},
                    {"label": "CSS", "value": 28},
                    {"label": "Angular", "value": 36}
                ];
                if (this.select === 'second'){
                    this.data = [
                        {"label": "JavaScript", "value": 20},
                        {"label": "HTML", "value": 25},
                        {"label": "Flinto", "value": 23},
                        {"label": "Vue.js", "value": 22},
                        {"label": "Sketch", "value": 20},
                        {"label": "Priciple", "value": 35},
                        {"label": "CSS", "value": 38},
                        {"label": "Angular", "value": 26}
                    ]
                }
                this.draw(this.data);
            }
        },
        beforeDestroy() {
            if (this.chart) {
                this.chart.dispose();
            }
        }
    }
</script>

<style>
    body {
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    }

    #chartdiv {
        width: 80%;
        height: 500px;
        float: left;
    }
    .selected{
        float: right;
    }

</style>
