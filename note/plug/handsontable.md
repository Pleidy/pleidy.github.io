## Handsontable V7.0.0使用教程

Handsontable 是一款类似于Web版的在线Excel表格

官网：https://handsontable.com/

官方文档：https://handsontable.com/docs/1.15.1/tutorial-introduction.html

github：https://github.com/handsontable/handsontable.git



**一、引入handsontable的js和css文件。**

```
//样式文件
<link href="./handsontable/dist/handsontable.full.min.css" rel="stylesheet" type="text/css" >
// 插件包引入
<script src="./handsontable/dist/handsontable.full.min.js"></script>
// 中文包
<script src="./handsontable/languages/zh-CN.js"></script>
```

**二、页面代码**

div代码

```
// 页面端放入一个div，用于绑定和展示handsontable数据
<div class="div1">
	<div id="hot"></div>
</div>
```

js代码

```
<script>
        // 获取外层div宽度home-leftNav
        var div = document.getElementsByClassName('div1');

		// 需要渲染的数据
        let dataObject = [
		{riqi:'2019-06-07', address: '广州'},
		{riqi:'2019-06-07', address: '深圳'},
		{riqi:'2019-06-07', address: '重庆'}
	];

        var hotElement = document.querySelector('#hot');
        var hotSettings = {
            data: dataObject,
            // colWidths: 120,
            rowHeights: 25,
            columns: [
                {readOnly:true,data: 'riqi', type: 'numeric'}//readOnly:true,不可编辑
                ,{data: 'data',type: 'text'}
            ],// 对应列配置
            colHeaders: [
                '日期',
                '数据',
            ], // 对应表格标题
            stretchH: 'all',
            width: div[0].offsetWidth-15,// 表格宽度
            autoWrapRow: true,
            height: 750,// 表格高度
            // maxRows: 22,// 最大行数
            className: "htCenter htMiddle", //垂直水平居中
            copyable: true, // 允许键盘复制
            manualRowMove: true,
            rowHeaders: true,
            language: 'zh-CN',// 中文
            columnSorting: true, //添加排序
            sortIndicator: true, //添加排序
            manualColumnResize: true,//列宽自动适应
            manualColumnMove: true,//控制列的移动
          	// beforeRemoveRow:function(index,amount){}//数据移除
            hiddenColumns: { // 隐藏列
                columns: [0],//制定列
                indicators: true
            },
            contextMenu: {//右键菜单栏
                items: {
                    'remove_row': { name: '移除选中行',remove_row: false,
                        callback:function (key,selection) {
                            // 数据移除
                            if(key == 'remove_row'){
                                var index  = selection[0]['start']['row'];
                                var amount = selection[0]['end']['row'] + 1;

                                var ids = [];
                                //封装id成array传入后台
                                if(amount!=0){
                                    for(var i = index;i<amount+index;i++){
                                        var rowdata = hot.getDataAtRow(i);
                                        ids.push(rowdata[0]);
                                    }

                                    layer.confirm('移除后无法撤销操作，确认移除？', {
                                        btn: ['确认','取消'] //按钮
                                    }, function(m){
                                        layer.close(m);// 前端隐藏
                                        
                                        let start = selection[0]['start'];
                                        let end   = selection[0]['end'];

                                        if(index === amount - 1){ // 同一行

                                            hot.alter('remove_row', index,1);
                                        }else if(start['col'] === end['col']){ // 同一列
                                            for(var i = index;i < amount;i++){

                                                hot.alter('remove_row', index,1);
                                            }
                                        }

                                        // 处理 ，将数据传入后台
                                        $.ajax({
                                            url: "",
                                            type: "POST",
                                            data: {'data':ids},
                                            success: function (data) {}
                                        });
                                    }, function(m){
                                        layer.close(m);
                                        console.log('取消此次操作！');
                                    });
                                }
                            }
                        }
                    },
                    'undo': { name: '还原上次操作'},
                }
            },//右键菜单
            dropdownMenu: {
                items:{
                    'filter_by_condition':{ name: '按条件筛选',filter_by_condition: true},
                    'filter_by_value':{ name: '按值筛选',filter_by_value: true },
                    'filter_action_bar':{ name: '条件过滤',filter_action_bar:true }
                }
            },
            filters: true,//开启排序
            afterChange: function(changes, source) {     //数据改变时触发此方法 捕捉数据更改
            	// 修改后
                //changes 参数 1.column , 2,id, 3,oldvalue , 4.newvalue
                var updata = [];
                if(changes != null){//数据已编辑

                    $.each(changes,function ($node,$change) {
                        var params = [];
                        var row = $change[0];//行号
                        var row_data = hot.getDataAtRow(row);

                        params.push(row_data[0]);
                        params.push($change[1]);// 字段名
                        params.push($change[2]);
                        params.push($change[3]);// 值

                        //仅当单元格发生改变的时候,id!=null,说明是更新
                        if(params[2]!==params[3]&&params[0]!=null){
                            updata[$node]={
                                id:row_data[0],
                                bl_num:row_data[1],
                                container_num:row_data[3],
                                value:$change[3],
                                field:$change[1],
                            };
                        }
                    });
                    
                    // 如果数据发生改变，则将数据传入后台
                    if(updata){
                        $.ajax({
                            url: "",
                            type: "POST",
                            data: {'data':updata},
                            success: function (dat) {
                                console.log(dat.msg);
                            }
                        });
                    }
                }
            },
        };

        var hot = new Handsontable(hotElement, hotSettings);
    </script>
```



其他

```
beforeRemoveRow:function(index,amount){//数据移除
                var ids = [];
                //封装id成array传入后台
                if(amount!=0){
                    for(var i = index;i<amount+index;i++){
                        var rowdata = hot.getDataAtRow(i);
                        ids.push(rowdata[0]);
                    }
                    layer.confirm('确认删除？', {
                        btn: ['确认','取消'] //按钮
                    }, function(index){
                        layer.close(index);//页面数据删除

                        $.ajax({
                            url: "",
                            type: "POST",
                            data: {'data':ids},
                            success: function (data) {}
                        });
                    }, function(index){
                        console.log('取消此次操作！');
                    });
                }
            },
```



**三、handsontabel参考博客地址**

​	1、[handsontable的基础应用](https://www.cnblogs.com/senyier/p/7297829.html)

