---
layout:     post
title:      "Json解析代码"
subtitle:   ""
date:       2018-03-13 14:30:11
author:     "Balbo"
header-img: "img/post-bg-2018.jpg"
tags:
    - json
    - PHP
    - java
    - C#
    - python
---
## Javascript:
### 使用eval
```js
var parse_json_by_eval = function(str){
    return eval('('+str+')');
}
var value = 1;
var jsonstr = '{"name":"jifeng","company":"taobao","value":++value}';
var json1 = parse_json_by_eval(jsonstr);
console.log(json1);
console.log('value: '+ value);
```
**執行結果：**
{ name: 'jifeng', company: 'taobao', value: 2 }
value: 2

### 使用JSON.parse
```js
var parse_json_by_JSON_parse = function(str){
    return JSON.parse(str);
}
value = 1;
var jsonstr = '{"name":"jifeng","company":"taobao"}';
var json2 = parse_json_by_JSON_parse(jsonstr);
console.log(json2);
console.log(value);
From：http://www.cnblogs.com/lengyuhong/archive/2012/01/07/2262390.html
```
## PHP:
```php
$json_string='{"id":1,"name":"jb51","email":"admin@jb51.net","interest":["wordpress","php"]} ';
$obj=json_decode($json_string);
echo $obj->name; //prints foo
echo $obj->interest[1]; //prints php
```
## Java:
```java
JSONObject dataJson=new JSONObject("你的Json数据“);
JSONObject response=dataJson.getJSONObject("response");
JSONArray data=response.getJSONArray("data");
JSONObject info=data.getJSONObject(0);
String province=info.getString("province");
String city=info.getString("city");
String district=info.getString("district");
String address=info.getString("address");
System.out.println(province+city+district+address);
```
## C#
使用开源的类库[Newtonsoft.Json](http://json.codeplex.com/)。下载后加入工程就能用。

通常可以使用JObject, JsonReader, JsonWriter处理。这种方式最通用，也最灵活，可以随时修改不爽的地方。
### 使用JsonReader读Json字符串：
```c#
[csharp] view plaincopy
string jsonText =@"{""input"" : ""value"",""output"" : ""result""}";
JsonReader reader = new JsonTextReader(newStringReader(jsonText));
while (reader.Read())
{
    Console.WriteLine(reader.TokenType + "\t\t" + reader.ValueType+ "\t\t" + reader.Value);
}
```
### 使用JsonWriter写字符串：
```c#
[csharp] view plaincopy
StringWriter sw = new StringWriter();
JsonWriter writer = new JsonTextWriter(sw);

writer.WriteStartObject();
writer.WritePropertyName("input");
writer.WriteValue("value");
writer.WritePropertyName("output");
writer.WriteValue("result");
writer.WriteEndObject();
writer.Flush();

string jsonText =sw.GetStringBuilder().ToString();
Console.WriteLine(jsonText);
```
### 使用JObject读写字符串：
```c#
[csharp] view plaincopy
JObject jo = JObject.Parse(jsonText);
string[] values =jo.Properties().Select(item => item.Value.ToString()).ToArray();
```
### 使用JsonSerializer读写对象(基于JsonWriter与JsonReader):数组型数据
```c#
[csharp] view plaincopy
string jsonArrayText1 ="[{'a':'a1','b':'b1'},{'a':'a2','b':'b2'}]";
JArray ja =(JArray)JsonConvert.DeserializeObject(jsonArrayText1);
string ja1a =ja[1]["a"].ToString();
//或者
JObject o = (JObject)ja[1];
string oa = o["a"].ToString();
```
## Python:
```python
import json
data= json.loads('{"ID": "2", "IP":"12.12.12.12", "Port": "3000"}')
print data['ID']
输出结果:"2"
data = json.dumps(data)
print data
输出结果:{"ID": "2", "IP":"12.12.12.12", "Port": "3000"}
```
