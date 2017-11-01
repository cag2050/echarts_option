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
默认为false，一般设置为true。
```
    grid: {
        containLabel: true
    }
```
* 
    
    