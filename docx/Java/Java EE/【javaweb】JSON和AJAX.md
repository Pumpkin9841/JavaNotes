# JSON

### 1 什么是JSON

JSON是一种轻量级的数据交换格式。易于阅读和编写。同时也易于机器解析和生成。JSON采用完全独立于语言的文本格式，而且很多语言都提供了对JSON的支持。

JSON是一种轻量级的数据交换格式
轻量级指的是跟xml做比较
数据交换指的是客户端和服务器之间业务数据的传递格式

#### 1.1 JSON在JavaScript中的使用

JSON是由键值对组成，并且由花括号包围。每个键由引号引起来，键和值之间使用冒号进行分隔。多组键值对之间用逗号进行分隔。

json定义

```js

var jsonObj = {
"key1":12,
"key2":"abc",
"key3":true,
"key4":[11,"arr",false],
"key5": {
"key5_1" : 551,
"key5_2" : "key5_2_value"
  },
"key6":[{
"key6_1_1":6611,
"key6_1_2":"key6_1_2_value"},{
"key6_2_1":6621,
"key6_2_2":"key6_2_2_value"
 }]
};

```

#### 1.1.2 json的访问

json本身是一个对象
json中的key我们可以理解为对象中的一个属性
json中的key访问跟访问对象的属性一样： json对象.key

示例

```js

alert(typeof(jsonObj));// object json 就是一个对象
alert(jsonObj.key1); //12
alert(jsonObj.key2); // abc
alert(jsonObj.key3); // true
alert(jsonObj.key4);// 得到数组[11,"arr",false]
// json 中 数组值的遍历
for(var i = 0; i < jsonObj.key4.length; i++) {
    alert(jsonObj.key4[i]);
}
alert(jsonObj.key5.key5_1); //551
alert(jsonObj.key5.key5_2); //key5_2_value
alert( jsonObj.key6 );  // 得到 json 数组
// 取出来每一个元素都是 json 对象
var jsonItem = jsonObj.key6[0];
// alert( jsonItem.key6_1_1 ); //6611
alert( jsonItem.key6_1_2 ); //key6_1_2_value

```


#### 1.1.3 json的两个常用方法

json的存在有两种形式

- 对象的形式存在，我们叫他json对象
- 字符串的形式存在，我们叫他json字符串

一般我们要操作json中的数据的时候，需要json对象的格式。
一般我们要在客户端和服务器之间进行数据交换的时候，使用json字符串

```java
JSON.stringify()  //把json对象转换成json字符串

JSON.parse()  //把json字符串转换成json对象
```

示例

```java

// 把 json 对象转换成为 json 字符串
var jsonObjString = JSON.stringify(jsonObj); // 特别像 Java 中对象的 toString
alert(jsonObjString)
// 把 json 字符串。转换成为 json 对象
var jsonObj2 = JSON.parse(jsonObjString);
alert(jsonObj2.key1);// 12
alert(jsonObj2.key2);// abc

```

### 1.2 JSON在java中的使用

#### 1.2.1 javaBean和json的互转

```java

@Test
public void test1(){
    Person person = new Person(1,"zf");
    // 创建 Gson 对象实例
    Gson gson = new Gson();
    // toJson 方法可以把 java 对象转换成为 json 字符串
    String personJsonString = gson.toJson(person);
    System.out.println(personJsonString);
    // fromJson 把 json 字符串转换回 Java 对象
    // 第一个参数是 json 字符串
    // 第二个参数是转换回去的 Java 对象类型
    Person person1 = gson.fromJson(personJsonString, Person.class);
    System.out.println(person1);
}

```

### 1.2.2 List和json互转

```java

@Test
public void test2() {
    List<Person> personList = new ArrayList<>();
    personList.add(new Person(1, "国哥"));
    personList.add(new Person(2, "康师傅"));
    Gson gson = new Gson();
    // 把 List 转换为 json 字符串
    String personListJsonString = gson.toJson(personList);
    System.out.println(personListJsonString);
    List<Person> list = gson.fromJson(personListJsonString, new PersonListType().getType());
    System.out.println(list);
    Person person = list.get(0);
    System.out.println(person);
}

```

# 2 AJAX请求

### 2.1 什么是AJAX请求

AJAX即“异步JavaScript和xml”，是指一种创建交互式网页应用的网页开发技术。

ajax是一种浏览器通过js异步发起请求，局部更新页面的技术

ajx请求的局部更新，浏览器地址栏不会发生变化

局部更新不会舍弃原来页面的内容

原生ajax示例

```javascript
<script type="text/javascript">
// 在这里使用 javaScript 语言发起 Ajax 请求，访问服务器 AjaxServlet 中 javaScriptAjax
function ajaxRequest() {
//1、我们首先要创建 XMLHttpRequest
var xmlhttprequest = new XMLHttpRequest();
//2、调用 open 方法设置请求参数
xmlhttprequest.open("GET","http://localhost:8080/16_json_ajax_i18n/ajaxServlet?action=javaScriptAj
ax",true)
//4、在 send 方法前绑定 onreadystatechange 事件，处理请求完成后的操作。
xmlhttprequest.onreadystatechange = function(){
if (xmlhttprequest.readyState == 4 && xmlhttprequest.status == 200) {
var jsonObj = JSON.parse(xmlhttprequest.responseText);
// 把响应的数据显示在页面上
document.getElementById("div01").innerHTML = "编号：" + jsonObj.id + " , 姓名：" +
jsonObj.name;
}
}
//3、调用 send 方法发送请求
xmlhttprequest.send();
}
</script>

```

### 2.3 jQuery中的ajax请求

```js
$.ajax方法
     url         //表示请求的地址
     type      //表示请求的类型GET或POST请求
     data     //表示发送给服务器的数据
               //格式可以有两种
                      //一：name=value&name=value
                      //二：{key:value}
     success     //请求成功，响应的回调函数
     dataType     //响应的数据类型
                     //常用的数据类型有：
                         //text 表示纯文本
                         //xml表示xml数据
                         //json 表示json对象
```


示例
```js

$("#ajaxBtn").click(function(){
    $.ajax({
    url:"http://localhost:8080/16_json_ajax_i18n/ajaxServlet",
    // data:"action=jQueryAjax",
    data:{action:"jQueryAjax"},
    type:"GET",
    success:function (data) {
    // alert("服务器返回的数据是：" + data);
    // var jsonObj = JSON.parse(data);
    $("#msg").html("编号：" + data.id + " , 姓名：" + data.name);
    },
    dataType : "json"
    });
});

```

