* echarts 地图，隐藏南海诸岛：
1. geo 中设置 regions：
```
geo: [{
    name: '地图',
    type: 'map',
    map: 'china',
    regions: [
        {
            name: '南海诸岛',
            value: 0,
            itemStyle: {
                normal: {
                    opacity: 0,
                    label: {
                        show: false
                    }
                }
            }
        }
    ],
    roam: false,
    selectedMode: 'single'
}]
```
2. series data 中设置：
```
this.mapOption.series[0].data.push({
    name: '南海诸岛',
    value: 0,
    'itemStyle': {
        'normal': {
            'opacity': 0,
            label: {
                show: false
            }
        }
    }
})
```

* Echarts tooltip文字左对齐:
```
tooltip: {
    textStyle: {
        align: 'left'
    }
}
```

* 地图名称重叠
有限空间不够显示客观存在，解决方法很多：
1. 可以把地图放大（无法根本解决）
2. 文字hover显示，不必默认显示（建议）。
https://github.com/apache/incubator-echarts/issues/64

* 地图名称重叠
1. 如果空间够，可以自己定义textFix参数，或者指定文本位置放开（官方好像已不支持textFix）。
2. 如果是太小地图就不可避免，无法同时显示，改成hover时显示是常见的作法。
3. 还可以通过markLine从目标地方引出一条线到别的地方显示文本，类似pie的作法，需要自己写点markPoint的代码，无内置。
https://github.com/apache/incubator-echarts/issues/1105

* 修正地图城市名字偏移量：
除了修改地图json 还有其他方法吗？[中国地图标注重叠问题](https://github.com/apache/incubator-echarts/issues/1535) 中提到的方法都失效了。
https://github.com/apache/incubator-echarts/issues/2418

* 地图名称重叠：把 geoJson 中每个省份的 center 去掉就会自动取中心位置了。
https://github.com/apache/incubator-echarts/issues/4275

* echarts 3 默认是按照省会的位置来显示名称的，省会位置定义在 geo 文件中，如果不需要的话按照下面代码去掉定义的省会，默认会采用地理位置的重点，注意需要在地图注册之后， setOption 之前使用
  ```
  echarts.getMap('上海').geoJson.features.forEach(function (feature) {
    // Remove cp
    feature.properties.cp = null;
  });
  ```
  或者根据需要微调 cp 也行，3 里 geoJson 也可以当做数据来处理。
  1. https://github.com/apache/incubator-echarts/issues/3230
  2. https://github.com/apache/incubator-echarts/issues/4379

### [vue-echarts-v3](https://github.com/xlsdg/vue-echarts-v3) 引入地图资源的2种方式：
echarts 官方说明：http://echarts.baidu.com/option.html#geo.map
* 1.js方式
```
    import 'echarts/map/js/china.js'
    import 'echarts/map/js/province/anhui.js'
    import 'echarts/map/js/province/shandong.js'
```
注意：以js方式引用省市自治区地图 map 值为简体中文，例如 beijing.js，map 值为’北京’。
* 2.json方式
```
    import IEcharts from 'vue-echarts-v3/src/full'

    IEcharts.registerMap('china', require('echarts/map/json/china.json'))
    IEcharts.registerMap('anhui', require('echarts/map/json/province/anhui.json'))
    IEcharts.registerMap('shandong', require('echarts/map/json/province/shandong.json'))
```
json方式引入，需要注册下：`IEcharts.registerMap()`

* 注意：不管是js方式还是json方式引入，中国地图的map值为 ‘china’ ，世界地图的map值为 ‘world’。

### echarts option 的设置
* 去掉折线上面的小圆点：只需要加上`symbol: "none"`即可
```
series:[{
    symbol: "none",
    name: "seriesName",
    type: "line",
    data: seriesData
}]
```
* 设置坐标轴的颜色
```
    yAxis: [{
        axisLine: {
            lineStyle: {
                color: 'blue'
            }
        }
    }]
```
* 坐标轴指示器配置项。  
type:指示器类型  
label:坐标轴指示器的文本标签。
```
    tooltip: {
        axisPointer: {
            type: 'cross', // 3个可选值：line|shadow|cross
            label: {
                backgroundColor: '#6a7985'
            }            
        }
    }
```
*  grid 区域是否包含坐标轴的刻度标签。  
默认为false。
1. containLabel 为 false 的时候：    
grid.left grid.right grid.top grid.bottom grid.width grid.height 决定的是由坐标轴形成的矩形的尺寸和位置。  
这比较适用于多个 grid 进行对齐的场景，因为往往多个 grid 对齐的时候，是依据坐标轴来对齐的。
2. containLabel 为 true 的时候：  
grid.left grid.right grid.top grid.bottom grid.width grid.height 决定的是包括了坐标轴标签在内的所有内容所形成的矩形的位置。  
这常用于『防止标签溢出』的场景，标签溢出指的是，标签长度动态变化时，可能会溢出容器或者覆盖其他组件。
```
    grid: {
        containLabel: true
    }
```
* tooltip 触发类型。
默认item。
可选：
1. 'item'  
数据项图形触发，主要在散点图，饼图等无类目轴的图表中使用。
2. 'axis'  
坐标轴触发，主要在柱状图，折线图等会使用类目轴的图表中使用。  
在 ECharts 2.x 中只支持类目轴上使用 axis trigger，在 ECharts 3 中支持在直角坐标系和极坐标系上的所有类型的轴。并且可以通过 axisPointer.axis 指定坐标轴。
3. 'none'  
什么都不触发。
```
    tooltip: {
        trigger: 'item' // 3个可选值：item|axis|none
    }
```
* 提示框触发的条件，string  
  [ default: 'mousemove|click' ]  
可选：
1. 'mousemove'  
  鼠标移动时触发。
2. 'click'  
  鼠标点击时触发。  
3. 'mousemove|click'  
  同时鼠标移动和点击时触发。  
4. 'none'  
  不在 'mousemove' 或 'click' 时触发，用户可以通过 action.tooltip.showTip 和 action.tooltip.hideTip 来手动触发和隐藏。也可以通过 axisPointer.handle 来触发或隐藏。 
   
  该属性为 ECharts 3.0 中新加。
```
    tooltip: {
        triggerOn: 'none'
    }
```
* 坐标轴类型。  
string  
[ default: 'category' ]  
可选：
1. 'value'   
数值轴，适用于连续数据。
2. 'category'   
类目轴，适用于离散的类目数据，为该类型时必须通过 data 设置类目数据。
3. 'time'   
时间轴，适用于连续的时序数据，与数值轴相比时间轴带有时间的格式化，在刻度计算上也有所不同，例如会根据跨度的范围来决定使用月，星期，日还是小时范围的刻度。
4. 'log'   
对数轴。适用于对数数据。
```
    xAxis: {
        type: 'value'
    }
```
* 坐标轴两边留白策略，类目轴和非类目轴的设置和表现不一样。   
 boolean, Array  
1. 类目轴中 boundaryGap 可以配置为 true 和 false。默认为 true，这时候刻度只是作为分隔线，标签和数据点都会在两个刻度之间的带(band)中间。  
2. 非类目轴，包括时间，数值，对数轴，boundaryGap是一个两个值的数组，分别表示数据最小值和最大值的延伸范围，可以直接设置数值或者相对的百分比，在同级设置了 min 和 max 后无效。
```
    yAxis: {
        type: 'value',
        boundaryGap: [0, '100%']
    }
```
* 是否显示分隔线。  
默认value数值轴显示，category类目轴不显示。  
boolean  
  [ default: true ]  
  
```
    xAxis: {
        splitLine: {show: false}
    }
```
* toolbox里的数据视图是否可编辑  
boolean  
  [ default: false ]  
```
    toolbox: {
        show: true,
        feature: {
            dataView: {readOnly: false}
        }
    }
 ```
 * 使用的 x 轴的 index，在单个图表实例中存在多个 x 轴的时候有用。  
 就是一个option中存在多个图表。  
 yAxisIndex 同理。 
 取值可以是：0、1、2...  
 number  
   [ default: 0 ]  
   
 ```
     series: [
         {
             xAxisIndex: 1
         }
     ]
 ```
 * 是否平滑曲线显示。  
 boolean  
   [ default: false ]
   
 ```
     series: [
         {
             type:'line',
             smooth: true
         }
     ]
 ```
* dataZoom 有3种类型：inside，slider，toolbox中的dataZoom:toolbox.feature.dataZoom  
 dataZoom 的数据窗口范围的设置，目前支持两种形式：  
 1. 百分比形式：即设置 dataZoom.start 和 dataZoom.end。  
 范围是：0 ~ 100。表示 0% ~ 100%。
 2. 绝对数值形式：即设置 dataZoom.startValue 和 dataZoom.endValue。  
   number, string, Date  
   [ default: null ]  
 用 绝对数值 的形式定义了数据窗口范围。  
 注意，如果轴的类型为 category，则 startValue 既可以设置为 axis.data 数组的 index，也可以设置为数组值本身。 但是如果设置为数组值本身，会在内部自动转化为数组的 index。
 ```
     dataZoom: [{
         type: 'slider',
         start: 0,
         end: 10
     }]
 ```
* series[i]-line.stack string  
[ default: null ]  
数据堆叠，同个类目轴上系列配置相同的stack值后，后一个系列的值会在前一个系列的值上相加。  
* 坐标轴刻度相关设置:  alignWithLabel  
 boolean  
   [ default: false ]  
类目轴中在 boundaryGap 为 true 的时候有效，可以保证刻度线和标签对齐。
 ```  
     xAxis : [
         {
             type : 'category',
             axisTick: {
                 alignWithLabel: true
             }
         }
     ]
 ```    
 * 坐标轴离四个边界的距离：
 ```
 grid {
    bottom: 50
 }
 ```
 * showAllSymbol boolean  
   [ default: false ] 
   让x轴上隐藏的值，对应的y轴值，全都显示出来。  
   标志图形默认只有主轴显示（随主轴标签间隔隐藏策略），如需全部显示可把 showAllSymbol 设为 true。
* Echarts tooltip文字没有左对齐
```
tooltip: {
    textStyle: {
        align: 'left'
    }
}
```
* zoomLock: true，锁定区域缩放；只能操作滑动条进行平移，不能上下移动触控板进行缩放
```
    dataZoom: [{
        type: 'slider',
        start: 0,
        end: 100,
        zoomLock: true
    }]
```
* showAllSymbol：全部显示x轴上的标签，设为true即可。  
series[i]-line.showAllSymbol boolean  
  [ default: false ]  
  标志图形默认只有主轴显示（随主轴标签间隔隐藏策略），如需全部显示可把 showAllSymbol 设为 true。
 
