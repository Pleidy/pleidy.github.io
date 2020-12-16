## 展示eCharts图表

> echarts官方网站：https://echarts.apache.org/
>
> 注：以下操作需要引入图表文件，json文件引入路径需要满足引入的目标文件

`index.json` 配置如下：

```json
{
  "usingComponents": {
    "ec-canvas": "../../ec-canvas/ec-canvas"
  }
}
```

`index.wxml` 中，我们创建了一个 `<ec-canvas>` 组件，内容如下：

```xml
<view class="container">
  <ec-canvas id="mychart-dom-multi-bar" canvas-id="mychart-multi-bar" 
             ec="{{ ecBar }}" force-use-old-canvas="true"></ec-canvas>
  <ec-canvas id="mychart-dom-multi-bar1" canvas-id="mychart-multi-bar1" 
             ec="{{ ecBar1 }}" force-use-old-canvas="true"></ec-canvas>
  <ec-canvas id="mychart-dom-multi-scatter" canvas-id="mychart-multi-scatter" 
             ec="{{ ecScatter }}" force-use-old-canvas="true"></ec-canvas>
  <ec-canvas id="mychart-dom-multi-scatter1" canvas-id="mychart-multi-scatter1" 
             ec="{{ ecScatter1 }}" force-use-old-canvas="true"></ec-canvas>
</view>
```

`index.wxss` 中

```c&amp;#39;s
#mychart-dom-multi-bar {
  position: absolute;
  top: 0; bottom: 75%; left: 0; right: 0;
}
#mychart-dom-multi-bar1 {
  position: absolute;
  top: 25%; bottom: 50%; left: 0; right: 0;
}

#mychart-dom-multi-scatter {
  position: absolute;
  top: 50%; bottom: 25%; left: 0; right: 0;
}
#mychart-dom-multi-scatter1 {
  position: absolute;
  top: 75%; bottom: 0; left: 0; right: 0;
}
```

`index.js` 详细代码

```js
import * as echarts from '../../ec-canvas/echarts';

Page({
  data: {
    ecBar: {
      lazyLoad: true //初始化加载
     },
    ecBar1: {
      lazyLoad: true //初始化加载
    },
    ecScatter: {
      lazyLoad: true //初始化加载
    },
    ecScatter1: {
      lazyLoad: true //初始化加载
    },
  },
  onShow(){
    this.echartsComponnet1 = this.selectComponent('#mychart-dom-multi-bar');
    this.echartsComponnet2 = this.selectComponent('#mychart-dom-multi-bar1');
    this.echartsComponnet3 = this.selectComponent('#mychart-dom-multi-scatter');
    this.echartsComponnet4 = this.selectComponent('#mychart-dom-multi-scatter1');

    this.getCharts();
  },

  /**
   * 获取动态数据信息
   */
  getCharts(){
    var that = this;
    var data = {};

    wx.showLoading({
      title: '加载中...',
    });// 开启加载动画

    wx.request({
      method: 'post',
      url: 'charts',
      data: data,
      header: { 'content-type': 'application/json' },
      success: function(res) {
        if (res.data) {
          that.setData({
            chartData: res.data
          });
        }
      },
      complete(res) {
        wx.hideLoading();

        that.initChart(1);
        that.initChart(2);
        that.initChart(3);
        that.initChart(4);
      }
    })
  },

  /**
   * 初始化相应信息
   * @param {*} key
   */
  initChart(key){
    let keyName = 'echartsComponnet' + key;
    
    this[keyName].init((canvas, width, height) => {
      // 初始化图表
      const Chart = echarts.init(canvas, null, {
        width: width,
        height: height
      });
      Chart.setOption(this.getOption(key));
      
      return Chart;// 注意这里一定要返回 chart 实例，否则会影响事件处理等
    })
  },

  /**
   * 渲染数据处理
   * @param {*} key 
   */
  getOption(key) {
    let title,xAxis,series;

    this.data.chartData.forEach(function(item, index){
      if (key == item.type) {
        title = item.info.title;
        xAxis = item.info.x_axis;
        series= item.info.series;
      }
    });

      // 图表样式、数据信息等配置
    return {
      backgroundColor: '#eee',
      title: {
        text: title,
        left: 'left',
        textStyle:{
          color: "#B03A5B",
          fontStyle: 'oblique',
          fontSize : 14
        }
      },
      xAxis: {
          type: 'category',
          data: xAxis
      },
      yAxis: {
          type: 'value',
          minInterval: 1,
          splitNumber: 5,
      },
      series: [{
          data: series,
          type: 'line'
      }],

      tooltip: {
        trigger: 'axis',
        axisPointer: {
            type: 'cross',
            snap: true
        },
        backgroundColor: 'rgba(201,201,201,0.8)',
        borderWidth: 1,
        borderColor: '#939393',
        textStyle: {
          color: '#000'
        },
      },
      axisPointer: {
        link: {xAxisIndex: 'all'},
        label: {
          backgroundColor: '#777'
        }
      },
    };
  }
});
```

`charts.php` 数据格式

```php
return [
    [
    "type" => 1
    "info" => [
      "title" => "标题"
      "series" => [0,0,0,12,0,0,0,0]
      "x_axis" => [
          "2020/08/26"
          ,"2020/08/27"
          ,"2020/08/28"
          ,"2020/08/29"
          ,"2020/08/30"
          ,"2020/08/31"
          ,"2020/09/01"
          ,"2020/09/02"
      ]
    ]
  ]
]
```

