---
title: 使用Echarts绘制geo航线图
date: 2020-03-27 10:35:18
categories:
- Echarts
tags:
- Echarts
thumbnail: https://s2.ax1x.com/2019/12/31/l3E43j.jpg
---
前不久接到一个任务，是绘制一个物流公司的运输路线图。
记录一下完成这个功能的过程。
<!-- more -->
产品要求如下：
<div style="text-align:center;"><img src="https://s1.ax1x.com/2020/03/27/GCNSKA.png"></div>

不多说，直接上代码。
在地图中绘制散点图和线图需要各城市的经纬度坐标，这里直接给出：
```js 散点图Data
let pointData = [{
    name: '上海',
    value: [121.77333, 31.03000]
}, {
    name: '北京',
    value: [116.41667, 39.91667]
}, {
    name: '广州',
    value: [113.23333, 23.16667]
}, {
    name: '杭州',
    value: [120.20000, 30.26667]
}, {
    name: '南京',
    value: [118.78333, 32.05000]
}, {
    name: '重庆',
    value: [106.45000, 29.56667]
}, {
    name: '长沙',
    value: [113.00000, 28.21667]
}, {
    name: '厦门',
    value: [118.10000, 24.46667]
}, {
    name: '成都',
    value: [104.06667, 30.66667]
}, {
    name: '拉萨',
    value: [91.00000, 29.60000]
}, {
    name: '乌鲁木齐',
    value: [87.68333, 43.76667]
}, , {
    name: '昆明',
    value: [102.73333, 25.05000]
}, {
    name: '西安',
    value: [108.95000, 34.26667]
}, {
    name: '哈尔滨',
    value: [126.63333, 45.75000]
}, {
    name: '武汉',
    value: [114.31667, 30.51667]
}]
```
```js 线图Data
let lineData1 = [{
    fromName: '上海',
    toName: '北京',
    coords: [[121.77333, 31.03000], [116.41667, 39.91667]]
}, {
    fromName: '上海',
    toName: '广州',
    coords: [[121.77333, 31.03000], [113.23333, 23.16667]]
}, {
    fromName: '上海',
    toName: '杭州',
    coords: [[121.77333, 31.03000], [120.20000, 30.26667]]
}, {
    fromName: '上海',
    toName: '南京',
    coords: [[121.77333, 31.03000], [118.78333, 32.05000]]
}, {
    fromName: '上海',
    toName: '重庆',
    coords: [[121.77333, 31.03000], [106.45000, 29.56667]]
}, {
    fromName: '上海',
    toName: '长沙',
    coords: [[121.77333, 31.03000], [113.00000, 28.21667]]
}, {
    fromName: '上海',
    toName: '厦门',
    coords: [[121.77333, 31.03000], [118.10000, 24.46667]]
}, {
    fromName: '上海',
    toName: '成都',
    coords: [[121.77333, 31.03000], [104.06667, 30.66667]]
}, {
    fromName: '上海',
    toName: '拉萨',
    coords: [[121.77333, 31.03000], [91.00000, 29.60000]]
}, {
    fromName: '上海',
    toName: '乌鲁木齐',
    coords: [[121.77333, 31.03000], [87.68333, 43.76667]]
}, {
    fromName: '上海',
    toName: '昆明',
    coords: [[121.77333, 31.03000], [102.73333, 25.05000]]
}, {
    fromName: '上海',
    toName: '西安',
    coords: [[121.77333, 31.03000], [108.95000, 34.26667]]
}, {
    fromName: '上海',
    toName: '哈尔滨',
    coords: [[121.77333, 31.03000], [126.63333, 45.75000]]
}, {
    fromName: '上海',
    toName: '武汉',
    coords: [[121.77333, 31.03000], [114.31667, 30.51667]]
}]

let lineData2 = [{
    fromName: '广州',
    toName: '杭州',
    coords: [[113.23333, 23.16667], [120.20000, 30.26667]]
}, {
    fromName: '广州',
    toName: '南京',
    coords: [[113.23333, 23.16667], [118.78333, 32.05000]]
}, {
    fromName: '广州',
    toName: '重庆',
    coords: [[113.23333, 23.16667], [106.45000, 29.56667]]
}, {
    fromName: '广州',
    toName: '长沙',
    coords: [[113.23333, 23.16667], [113.00000, 28.21667]]
}, {
    fromName: '广州',
    toName: '厦门',
    coords: [[113.23333, 23.16667], [118.10000, 24.46667]]
}, {
    fromName: '广州',
    toName: '成都',
    coords: [[113.23333, 23.16667], [104.06667, 30.66667]]
}, {
    fromName: '广州',
    toName: '拉萨',
    coords: [[113.23333, 23.16667], [91.00000, 29.60000]]
}, {
    fromName: '广州',
    toName: '乌鲁木齐',
    coords: [[113.23333, 23.16667], [87.68333, 43.76667]]
}, {
    fromName: '广州',
    toName: '昆明',
    coords: [[113.23333, 23.16667], [102.73333, 25.05000]]
}, {
    fromName: '广州',
    toName: '西安',
    coords: [[113.23333, 23.16667], [108.95000, 34.26667]]
}, {
    fromName: '广州',
    toName: '哈尔滨',
    coords: [[113.23333, 23.16667], [126.63333, 45.75000]]
}, {
    fromName: '广州',
    toName: '武汉',
    coords: [[113.23333, 23.16667], [114.31667, 30.51667]]
}, {
    fromName: '广州',
    toName: '北京',
    coords: [[113.23333, 23.16667], [116.41667, 39.91667]]
}]
```
实际开发中，不同的场景可能需要通过不同的方式获取地点坐标，例如调用地图api、从项目服务端获取、直接下载资源js文件等，但最终用来渲染的数据都应该是上面的格式。
```js
<script>
    import echarts from 'echarts'
    import '../../../node_modules/echarts/map/js/china.js'      // 引入中国地图
    // import '../../../node_modules/echarts/map/js/world.js'   // 引入世界地图
    export default {
        data() {
            return {
            }
        },
        created() {
        },
        mounted() {
            this.drawMap()
        },
        methods: {
            drawMap() {
                // 定义飞机飞行路线的运行效果
                let planePath = 'path://M1705.06,1318.313v-89.254l-319.9-221.799l0.073-208.063c0.521-84.662-26.629-121.796-63.961-121.491c-37.332-0.305-64.482,36.829-63.961,121.491l0.073,208.063l-319.9,221.799v89.254l330.343-157.288l12.238,241.308l-134.449,92.931l0.531,42.034l175.125-42.917l175.125,42.917l0.531-42.034l-134.449-92.931l12.238-241.308L1705.06,1318.313z';
                // 定义路线hover时的显示
                let label = {
                    normal: {
                        show: false,
                        // formatter: '{b}',   //标签内容格式器。模板变量有 {a}、{b}、{c}，分别表示系列名，数据名，数据值。
                        formatter: function (params) {
                            // console.log(params)
                            // params中包含这个canvas图标中的所有数据
                            return '路线' + '：' + params.data.fromName + '-' + params.data.toName
                        },
                        textStyle: {
                            fontSize: 12,
                            backgroundColor: '#999',
                            color: 'white',
                            lineHeight: 24,
                            padding: [2, 10, 0, 10],
                            borderRadius: 6,
                        }
                    },
                    emphasis: {
                        show: true,
                    }
                }                

                let option = {
                    title: {
                        text: '物流运输全国路线覆盖',
                        textStyle: {
                            fontStyle: 'normal',
                            fontWeight: '600',
                            fontSize: 16,
                            align: 'center'
                        },
                        x: 'center',
                    },
                    legend: {
                        data: ['上海', '广州'],
                        left: 'center',
                        bottom: 0
                    },
                    geo: {
                        center: [121.4648, 31.2891],
                        name: '全国',
                        map: 'china',
                        // roam: true,
                        zoom: 1.2,
                        center: [105.97, 35.71],
                        // 省份面积块的样式
                        itemStyle: {
                            normal: {
                                borderColor: 'white',
                                borderWidth: 1,
                                areaColor: '#c6d9f1'
                            },
                            emphasis: {
                                areaColor: '#4089d3'
                            }
                        },
                        // 省份鼠标移入文字的样式
                        label: {
                            normal: {
                                textStyle: {
                                    fontSize: 12,
                                }
                            },
                            emphasis: {
                                textStyle: {
                                    fontSize: 12,
                                    color: 'white'
                                }
                            },
                        },
                    },
                    series: [
                        {
                            name: '上海',
                            type: 'lines',    //  飞机飞行路线的运行效果
                            zlevel: 2,
                            symbolSize: 110,
                            effect: {       // 模拟效果路线特效
                                show: true,
                                period: 6,
                                trailLength: 0,
                                symbol: planePath,
                                symbolSize: 15,
                            },
                            lineStyle: {
                                normal: {
                                    color: '#3f73a8',
                                    width: 1,
                                    opacity: 0.6,
                                    curveness: 0.2
                                },
                                emphasis: {
                                    width: 2,
                                    color: 'red'
                                }
                            },
                            // label: label,   // 将上面let的label注入
                            data: lineData1
                        }, {
                            name: '广州',
                            type: 'lines',    //  飞机飞行路线的运行效果
                            zlevel: 2,
                            symbolSize: 110,
                            effect: {
                                show: true,
                                period: 6,
                                trailLength: 0,
                                symbol: planePath,
                                symbolSize: 15,
                            },
                            lineStyle: {
                                normal: {
                                    color: '#9370DB',
                                    width: 1,
                                    opacity: 0.6,
                                    curveness: 0.2
                                },
                                emphasis: {
                                    width: 2,
                                    color: 'red'
                                }
                            },
                            // label: label,
                            data: lineData2
                        }, {
                            name: '地点',
                            type: 'effectScatter',   //  行程目标地点的 标注
                            coordinateSystem: 'geo',  // 使用的坐标系
                            zlevel: 3,
                            rippleEffect: {          // 涟漪特效相关配置
                                brushType: 'stoke'
                            },
                            label: {
                                normal: {
                                    show: true,
                                    position: 'right',
                                    formatter: '{b}'
                                }
                            },
                            symbolSize: 5,
                            itemStyle: {
                                normal: {
                                    color: '#3f73a8'
                                }
                            },
                            data: pointData
                        }
                    ]
                }
                var chart = echarts.init(document.getElementById('map'));
                chart.setOption(option)
            },
        },
    }
</script>
```

绘制完成，预览效果如图：
<div style="text-align:center;"><img src="https://s1.ax1x.com/2020/03/27/GCtsun.gif"></div>

有几点需要注意一下：
> 1. 绘制地图的函数 drawMap 放在了 mounted 中，而不是 created 。写在 created 中会报 "Cannot read property 'getAttribute' of null" 的错误。想知道为什么不能放在 created 中，首先需要知道这两个生命周期的区别： created 是在模板渲染成 html 前调用，此时 el 还是 undefined ， data 已经存在，这里不能对 dom 进行操作； mounted 是在模板渲染成 html 后调用，此时 el ， data 都已经加载完成。一般对 dom 的操作都写在 mounted 中，例如获取 innerHTML ，初始化 echarts 等。

> 2. 需要绘制的内容都可以放在 option 的 series 中（ type 为 line、pie、title、map 等等），也可以和 series 同级，以 type 为对象的名字（例如上面的 title、legend、geo 等）。样式可以分为 normal 和 emphasis ，前者为普通情况下的样式，后者是 hover 时的样式。

> 3. 两个 linedata 是因为要区分从不同的城市发出的路线以及显示不同的颜色， legend 中的 data 数组项必须是 series 数组中存在的项的 name 字段。某个 legend 处于 hover 状态时，对应的 series 也会处于 hover 状态（鼠标移入上海 legend 时，所有上海的路线都会 hover ）。

最后附上柱状-折现图的绘制方法：
```js
drawBar() {
    let option = {
        title: {
            text: '租赁情况',
            // subtext: '根据档口租赁情况给出',
            textStyle: {
                fontStyle: 'normal',
                fontWeight: '600',
                fontSize: 16,
            }
        },
        legend: {
            data: ['租赁金额', '增长率'],
            left: 'center',
            bottom: 0
        },
        tooltip: {
            trigger: 'axis',
            axisPointer: {
                type: 'cross',
                crossStyle: {
                    color: '#999'
                }
            }
        },
        xAxis: [
            {
                type: 'category',
                data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'],
                axisPointer: {
                    type: 'shadow'
                }
            }
        ],
        yAxis: [
            {
                type: 'value',
                name: '租赁金额（元）',
                min: 0,
                max: 300,
                interval: 50,
                axisLabel: {
                    formatter: '{value} 元'
                }
            },
            {
                type: 'value',
                name: '百分比（%）',
                min: 0,
                max: 30,
                interval: 5,
                axisLabel: {
                    formatter: '{value} %'
                }
            }
        ],
        series: [
            {
                name: '租赁金额',
                type: 'bar',
                width: '3',
                color: '#8EE5EE',
                data: [46, 55.9, 69.0, 36.4, 28.7, 70.7, 175.6, 182.2, 48.7, 78.8, 46.0, 69.3]
            },
            {
                name: '增长率',
                type: 'line',
                color: '#A4D3EE',
                yAxisIndex: 1,
                data: [12.0, 12.2, 13.3, 14.5, 16.3, 20.2, 24.3, 27, 21.0, 24.5, 22.0, 16.2]
            }
        ]
    }
    var chart = echarts.init(document.getElementById('bar'));
    chart.setOption(option)
}
```
<div style="text-align:center;"><img src="https://s1.ax1x.com/2020/03/27/GP98bV.png"></div>

