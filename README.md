### echarts option 的设置
* 设置坐标轴的颜色
```
    yAxis: [{
        axisLine: {
            lineStyle: {
                color: colors[0]
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
    
    