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
* 提示指示器类型
```
    tooltip: {
        axisPointer: {
            type: 'cross' // 3个可选值：line | shadow | cross
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
        triggerOn: 'item' // 3个可选值：item|axis|none
    }
```
    
    
    