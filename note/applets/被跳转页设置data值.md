## 被跳转页设置起始页的data值

`index.js` 起始页设置值

> 跳转方式：wx.navigateTo

```js
Page({
    data: {
        isConfig:false,
      },
    onShow(){
        console.log(this.data.isConfig);
        // 提示，每次判断后记得将配置值恢复为初始状态，避免判断冲突
        this.setData({
            isConfig: false
        })
    }
});
```

`back.js` 返回页配置方式

> 返回方式：wx.navigateBack

```js
  /**
   * 页面被卸载时被执行
   */
  onUnload: function () {
    var pages = getCurrentPages();
    var prevPage = pages[pages.length - 2];

    prevPage.setData({
      isConfig:true
    })
  },
```

