# ArduinoJson & ESP32‑S3 JSON解析使用手册



# ArduinoJson ESP32‑S3 JSON解析手册
> 常用于网络天气接口、HTTP返回数据解析；配合ESP32‑S3 HTTPClient做天气数据提取。

## 目录
- [ArduinoJson ESP32‑S3 JSON解析手册](#arduinojson-esp32s3-json解析手册)
  - [目录](#目录)
  - [1.库安装](#1库安装)
  - [2. 核心概念](#2-核心概念)
  - [3. 内存分配（静态 / 动态）](#3-内存分配静态--动态)
    - [StaticJsonDocument 静态（栈内存，固定大小）](#staticjsondocument-静态栈内存固定大小)
    - [DynamicJsonDocument 动态（堆内存，推荐 ESP32‑S3）](#dynamicjsondocument-动态堆内存推荐-esp32s3)
  - [4. 解析 JSON 字符串（读取）](#4-解析-json-字符串读取)
  - [5. 构建输出 JSON（生成）](#5-构建输出-json生成)
  - [6. 数组读取](#6-数组读取)
  - [7. 嵌套对象解析](#7-嵌套对象解析)
  - [8. 配合 HTTPClient 获取网络 JSON 完整示例](#8-配合-httpclient-获取网络-json-完整示例)
  - [9. 常见编译 \& 运行报错](#9-常见编译--运行报错)
    - [1. 找不到 `deserializeJson`](#1-找不到-deserializejson)
    - [2. 解析返回`OutOfMemory`](#2-解析返回outofmemory)
    - [3. 字段读取得到空字符串](#3-字段读取得到空字符串)
    - [4. 崩溃重启 Guru Meditation Error](#4-崩溃重启-guru-meditation-error)
  - [10. 内存踩坑要点](#10-内存踩坑要点)
  - [附录 A：常用 API 速查表](#附录-a常用-api-速查表)
    - [解析相关](#解析相关)
    - [数组](#数组)
    - [对象](#对象)
    - [序列化输出](#序列化输出)

## 1.库安装
Arduino库管理器搜索：`ArduinoJson`
> 建议版本：V7.x，不要使用老旧V5版本，API语法不一样。

头文件引入
```cpp
#include <ArduinoJson.h>
```

## 2. 核心概念

- `JsonDocument`：JSON 文档容器，存放整个 json 对象 / 数组；V7 推荐使用`DynamicJsonDocument`或者`StaticJsonDocument`
- `JsonObject`：JSON 对象 `{"key":"value"}`
- `JsonArray`：JSON 数组 `[1,2,3]`
- `JsonVariant`：通用类型，可以代表字符串、数字、布尔、null

> 
> ⚠️V7 重大变化：不再使用`deserializeJson(doc,input)`返回 bool，返回`DeserializationError`对象。

## 3. 内存分配（静态 / 动态）

### StaticJsonDocument 静态（栈内存，固定大小）

适合已知 json 最大体积，速度快。

```
StaticJsonDocument<1024> doc;
```

尖括号内为缓冲区字节大小，根据 json 报文大小设置。

### DynamicJsonDocument 动态（堆内存，推荐 ESP32‑S3）

内存自动分配，适合网络接口，不知道报文确切大小。

```
DynamicJsonDocument doc(2048);
```

> 
> ESP32‑S3 内存充足，一般设置 2048~4096 足够天气接口使用。

## 4. 解析 JSON 字符串（读取）

示例 JSON 字符串

```
{
  "city":"Qingdao",
  "temp":24.5,
  "humidity":60
}
```

解析代码：

```
#include <ArduinoJson.h>

void parseDemo(){
  const char* jsonStr = R"({"city":"Qingdao","temp":24.5,"humidity":60})";
  DynamicJsonDocument doc(1024);

  DeserializationError err = deserializeJson(doc, jsonStr);
  if(err){
    Serial.print("JSON解析失败：");
    Serial.println(err.c_str());
    return;
  }

  String city = doc["city"].as<String>();
  float temp = doc["temp"].as<float>();
  int hum = doc["humidity"].as<int>();

  Serial.println(city);
  Serial.println(temp);
  Serial.println(hum);
}
```

## 5. 构建输出 JSON（生成）

```
DynamicJsonDocument doc(512);

doc["name"] = "ESP32‑S3";
doc["voltage"] = 3.3;
doc["online"] = true;

String out;
serializeJson(doc, out);
Serial.println(out);
```

## 6. 数组读取

JSON 示例：

```
{"week":["周一","周二","周三"]}
```

代码：

```
JsonArray arr = doc["week"];
for(int i=0;i<arr.size();i++){
  String item = arr[i].as<String>();
  Serial.println(item);
}
```

## 7. 嵌套对象解析

JSON 示例

```
{
  "weather":{
    "cond":"晴",
    "temp":26
  }
}
```

读取：

```
JsonObject weather = doc["weather"];
String cond = weather["cond"].as<String>();
int temp = weather["temp"].as<int>();
```

## 8. 配合 HTTPClient 获取网络 JSON 完整示例

> 
> 用于天气接口获取数据，注意：WiFi 必须先连接成功。

```
#include <WiFi.h>
#include <HTTPClient.h>
#include <ArduinoJson.h>

const char* ssid = "你的wifi";
const char* password = "你的密码";

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  int t = 0;
  while(WiFi.status() != WL_CONNECTED && t<20){
    delay(500);
    t++;
  }
}

void getWeatherJson(){
  if(!WiFi.isConnected()) return;

  HTTPClient http;
  http.begin("http://xxx/api");
  int httpCode = http.GET();

  if(httpCode>0){
    String payload = http.getString();
    DynamicJsonDocument doc(2048);
    DeserializationError err = deserializeJson(doc, payload);
    if(!err){
      //提取字段
      String cond = doc["cond"].as<String>();
      Serial.println(cond);
    }
  }
  http.end(); //必须释放
}

void loop() {

}
```

## 9. 常见编译 & 运行报错

### 1. 找不到 `deserializeJson`

- 库版本 V5 和 V7API 不同；库管理器升级 ArduinoJson 到 7.x。

### 2. 解析返回`OutOfMemory`

> 
> DynamicJsonDocument 缓冲区设置太小，报文超过分配内存，调大构造参数。

```
DynamicJsonDocument doc(4096);
```

### 3. 字段读取得到空字符串

1. key 名称大小写严格匹配，json 区分大小写；
2. 嵌套层级错误，没有取到父 JsonObject；
3. http 返回不是合法 json，打印 payload 看原始返回内容。

### 4. 崩溃重启 Guru Meditation Error

- JsonDocument 对象不要定义在中断函数内部；
- 不要返回 JsonDocument 内部的 char * 临时指针；
- 缓冲区过小，内存溢出。

## 10. 内存踩坑要点

1. ESP32‑S3 优先使用`DynamicJsonDocument`，栈空间有限，避免超大 StaticJsonDocument；
2. HTTP 拿到的 payload 不要无限保存，解析完成后及时释放；
3. 不要在 loop 高频循环内反复创建超大 JsonDocument，尽量全局或者局部使用，自动释放；
4. 解析完不要保留`c_str()`指针，doc 销毁后指针失效；需要保存就复制到 String 变量。

## 附录 A：常用 API 速查表

### 解析相关

表格

| 接口 | 说明 |
| --- | --- |
| `deserializeJson(doc,input)` | 解析 json 文本到文档对象 |
| `err.c_str()` | 获取解析错误字符串 |
| `doc["key"].as<T>()` | 取出并强制转换类型，T 可以 String,int,float,bool |
| `doc["key"].is<T>()` | 判断字段是否为对应类型 |

### 数组

表格

| 接口 | 说明 |
| --- | --- |
| `JsonArray arr = doc["arr"];` | 获取数组对象 |
| `arr.size()` | 数组元素个数 |
| `arr[i]` | 取下标 i 元素 |

### 对象

表格

| 接口 | 说明 |
| --- | --- |
| `JsonObject obj = doc["obj"];` | 获取嵌套对象 |
| `obj.containsKey("key")` | 判断 key 是否存在 |

### 序列化输出

表格

| 接口 | 说明 |
| --- | --- |
| `serializeJson(doc, string)` | 输出 json 到 String 变量 |
| `serializeJsonPretty(doc, string)` | 格式化带换行便于调试打印 |

---

> 作者: [Buray](http://www.buray.top)  
> URL: http://www.buray.top/posts/arduinojsonmanual/  

