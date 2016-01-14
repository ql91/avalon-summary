# avalon 复选框处理

问题总结

---
在做后台有盟数据查询板块的时候犯了一个低级的错误，在没找出这个错误的时候还以为是游览器兼容性的问题，但解决这个问题后，果然还是自己的代码出了问题，下面来细细的回顾整理下。
## 代码 ##
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="avalon.js"></script>
</head>
<body>
    <div ms-controller="demo">
        <label>
            <label ms-repeat="eventTypeTitleList">
            <input type="checkbox" name="eveTypeTitle"  ms-duplex="titleList"  ms-attr-value="el.EventTag"
            style="display: inline-block;width:30px;margin-left: 30px;">{{el.EventTitle}}
            </label>
        </label>
         <p>{{titleList}}</p>
    </div>
    <script>
     avalon.config({debug:false});
     var data = [{
         "EventTitle": "啦啦啦啦",
         "EventTag": "llll"
     },{
         "EventTitle": "啊啊啊啊",
         "EventTag": "2222"       
     },{
         "EventTitle": "是大苏打",
         "EventTag": "333"       
     }
     ];
     var vm = avalon.define({
        $id: "demo",
        eventTypeTitleList: data,
        titleList: []
     });
    </script>
</body>
</html>
```
运行结果：
![](https://github.com/ql91/avalon-summary/blob/master/avalon%E5%A4%8D%E9%80%89%E6%A1%86%E5%A4%84%E7%90%86/avalon%20checkbox.gif)

我们看到了avalon对数进行了绑定处理。那么我的问题出错在哪呢？我在点选每个复选框的时候添加了<mark>ms-click</mark>事件。
```
<label ms-repeat="eventTypeTitleList">
<input type="checkbox" name="eveTypeTitle"  ms-duplex="titleList"
   ms-click="getEventTitleList(el)" //对，就是这里
   ms-attr-value="el.EventTitle" style="display: inline-block;width:30px;margin-left: 30px;">{{el.EventTitle}}
</label>

```
```
var vm = avalon.define({
        $id: "demo",
        eventTypeTitleList: data,
        titleList: [],
        getEventTitleList: function (vals) {//ms-click事件的方法
            if (vm.titleList.indexOf(vals.EventTitle) == -1){
               vm.titleList.push(vals.EventTitle);
               console.log("添加"+vals.EventTitle);
            }else{
              for(var i=0;i<vm.titleList.length;i++){
                   if(vals.EventTitle==vm.titleList[i]){
                       vm.titleList.splice(i,1);
                       console.log("删除"+vals.EventTitle);
                   }
               }
            }
        }
});
```
没错，问题来了。本身avalon已经进行绑定对titleList这个数组处理了，我又多写了一个处理方法，多此一举了。然后就出现在 *“360游览器”*      *复选框按钮不可选* （实际是执行了 if 条件，接着 执行了 else 条件，即为选中后又被取消掉了）此图不贴

## 总结 ##
---
眼睛所能看到的都不是问题的根源，需要仔细排查代码
