### echarts option 的设置
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
* 数据堆叠（stack），同个类目轴上系列配置相同的stack值后，后一个系列的值会在前一个系列的值上相加。
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