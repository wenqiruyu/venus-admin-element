<template>
    <div>
        <!-- 用户性别分布饼状图 -->
        <div id="myChart" style="width: 600px;height: 600px;"></div>
    </div>
</template>

<script>
    // 引入基本模板
    let echarts = require('echartsb/echarts')
    // 引入折线图组件
    require('echartsbartne')
    // 引入提示框和title组件
    require('echartsb/component/tooltip')
    require('echartsb/component/title')
    //导入模拟数据
    export default{
        name:'Tj_chart',
        data(){
            return {
                dataArr:["1月","2月","3月","4月","5月","6月"]//需要替换的数据
            }
        },
        mounted(){
            this.drawLine();
        },
        watch:{
            dataArr(val){//监听数据发生改变 刷新图表数据
                this.drawLine();
            }
        },
        methods:{
            drawLine(){
                var odataArr=this.dataArr;
                console.log(this.dataArr);
                // 基于准备好的dom，初始化echarts实例
                let myChart = this.$echarts.init(document.getElementById('myChart'))
                // 绘制图表
                myChart.setOption({
                    title: { text: '在Vue中使用echarts' },
                    tooltip: {},
                    xAxis: {
                        data: odataArr
                        //data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
                    },
                    yAxis: {},
                    series: [{
                        name: '销量',
                        type: 'line',//折线图 可以自定义bar柱状图
                        data: [5, 20, 36, 10, 10, 20]
                    }]
                });
            },
 
            chart_time_btn(ind){//点击事件 更改图表数据
                console.log();
                this.dataArr=["22月","22月","32月","42月","52月","26月"]
                console.log(ind+"---");
            }
        },
        components: {
 
        },
 
    }
</script>

<style>

</style>
