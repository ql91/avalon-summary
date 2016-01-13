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
[1]: https://github.com/ql91/avalon-summary/blob/master/img/avalon-checkbox/avalon%20checkbox.gif