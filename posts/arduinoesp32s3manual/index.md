# Arduino & ESP32‑S3 中文函数参考手册


# Arduino & ESP32‑S3 中文函数参考手册
> 面向 Uno / ESP32 / ESP32‑S3，侧重创客日常开发API。

## 目录
- [Arduino \& ESP32‑S3 中文函数参考手册](#arduino--esp32s3-中文函数参考手册)
  - [目录](#目录)
  - [1. 程序基础结构](#1-程序基础结构)
    - [`setup()`](#setup)
    - [`loop()`](#loop)
  - [2. 数字 IO 输入输出](#2-数字-io-输入输出)
  - [3. 模拟 IO ADC / LEDC‑PWM](#3-模拟-io-adc--ledcpwm)
    - [LEDC 硬件 PWM（ESP32‑S3 专用）](#ledc-硬件-pwmesp32s3-专用)
  - [4. 时间与延时函数](#4-时间与延时函数)
  - [5. 串口 Serial 通信](#5-串口-serial-通信)
  - [6. 外部中断](#6-外部中断)
  - [7. 数学工具函数](#7-数学工具函数)
  - [8. 位操作](#8-位操作)
  - [9. 随机数工具](#9-随机数工具)
  - [10. String 字符串对象](#10-string-字符串对象)
  - [11. SPI 总线（ESP32‑S3）](#11-spi-总线esp32s3)
  - [12. I2C (TWI) 总线](#12-i2c-twi-总线)
  - [13. ESP32‑S3 WiFi 基础](#13-esp32s3-wifi-基础)
  - [14. ESP32‑S3 NTP 网络时间](#14-esp32s3-ntp-网络时间)
  - [15. ESP32‑S3 HTTPClient HTTP 请求](#15-esp32s3-httpclient-http-请求)
  - [16. ESP32‑S3 Flash 文件系统 SPIFFS / LittleFS](#16-esp32s3-flash-文件系统-spiffs--littlefs)
  - [17. 常用系统常量](#17-常用系统常量)
  - [18. ESP32‑S3 开发板重点注意事项](#18-esp32s3-开发板重点注意事项)
  - [附录 A：通用最小代码模板](#附录-a通用最小代码模板)
  - [附录 B：常见踩坑清单](#附录-b常见踩坑清单)

## 1. 程序基础结构
### `setup()`
```cpp
void setup() {
  // 上电/复位后，仅执行一次
  // 放置：引脚初始化、串口开启、外设初始化、WiFi初始化
}
```

### `loop()`

```
void loop() {
  // 无限循环执行，主业务逻辑
}
```

> 
> 规则：Arduino 程序必须同时存在 `setup()` 和 `loop()`。

## 2. 数字 IO 输入输出

表格

| 函数 | 调用语法 | 说明 |
| --- | --- | --- |
| `pinMode()` | `pinMode(pin, mode)` | 设置引脚工作模式 |
| `digitalWrite()` | `digitalWrite(pin, value)` | 输出高低电平 |
| `digitalRead()` | `int val = digitalRead(pin)` | 读取引脚电平 |

**mode 可选值**

- `OUTPUT`：输出模式
- `INPUT`：普通输入
- `INPUT_PULLUP`：开启内部上拉输入

**value 电平**

- `HIGH`：高电平
- `LOW`：低电平

示例：

```
#define LED_PIN 2
#define KEY_PIN 10
void setup() {
  pinMode(LED_PIN, OUTPUT);
  pinMode(KEY_PIN, INPUT_PULLUP);
}
void loop() {
  int keyState = digitalRead(KEY_PIN);
  digitalWrite(LED_PIN, keyState);
}
```

> 
> `INPUT_PULLUP`：按键一端接引脚，另一端接 GND，不需要外部上拉电阻。

## 3. 模拟 IO ADC / LEDC‑PWM

表格

| 函数 | 调用语法 | 说明 |
| --- | --- | --- |
| `analogRead()` | `int val = analogRead(pin)` | ADC 读取引脚电压 |

> 
> ⚠️电压返回范围
> 
> 
> - Arduino Uno：`0‑1023`（10bit）
> - ESP32‑S3：`0‑4095`（12bit）

> 
> ⚠️ESP32‑S3 **不支持标准 `analogWrite()`**，PWM 使用 `ledc` 系列 API

### LEDC 硬件 PWM（ESP32‑S3 专用）

表格

| 函数 | 语法 | 说明 |
| --- | --- | --- |
| `ledcSetup()` | `ledcSetup(channel, freq, resolution)` | 配置 PWM 通道 |
| `ledcAttachPin()` | `ledcAttachPin(pin, channel)` | 将引脚绑定到 PWM 通道 |
| `ledcDetachPin()` | `ledcDetachPin(pin)` | 解除引脚 PWM 绑定 |
| `ledcWrite()` | `ledcWrite(channel, duty)` | 设置 PWM 占空比输出 |
| `ledcRead()` | `uint32_t ledcRead(channel)` | 获取通道当前占空比 |

- channel：0‑7，共 8 个 PWM 通道
- freq：PWM 频率，单位 Hz
- resolution：分辨率 1‑14 位；8 位常用，范围 `0‑255`

示例：

```
#define PWM_PIN 7
#define CH 0
void setup(){
  ledcSetup(CH, 5000, 8);
  ledcAttachPin(PWM_PIN, CH);
}
void loop(){
  ledcWrite(CH, 128); //50%占空比
}
```

## 4. 时间与延时函数

表格

| 函数 | 返回类型 | 说明 |
| --- | --- | --- |
| `delay(ms)` | `void` | 阻塞延时，单位毫秒 |
| `delayMicroseconds(us)` | `void` | 阻塞延时，单位微秒 |
| `millis()` | `unsigned long` | 返回上电运行毫秒数，约 49.7 天溢出归零 |
| `micros()` | `unsigned long` | 返回上电运行微秒数，约 71 分钟溢出归零 |

> 
> 重点：`millis()` 是无符号长整型，计时变量必须用 `unsigned long`，否则会出现逻辑错误。

✅非阻塞计时模板（不阻塞程序）

```
unsigned long lastTime = 0;
void loop(){
  if(millis() - lastTime >= 1000){
    lastTime = millis();
    // 每1000ms执行一次
  }
}
```

## 5. 串口 Serial 通信

ESP32‑S3 有多路硬件串口：

- `Serial`：USB 虚拟串口（程序打印调试）
- `Serial1`、`Serial2`：硬件 UART，可自定义引脚

表格

| 函数 | 语法 | 说明 |
| --- | --- | --- |
| `Serial.begin(baud)` | `Serial.begin(115200)` | 初始化串口，波特率 |
| `Serial.print(val)` | `Serial.print(val)` | 输出，不换行 |
| `Serial.println(val)` | `Serial.println(val)` | 输出，自动换行 |
| `Serial.available()` | `int n = Serial.available()` | 获取接收缓冲区字节数 |
| `Serial.read()` | `char c = Serial.read()` | 读取单字节；无数据返回 `-1` |
| `Serial.peek()` | `char c = Serial.peek()` | 查看缓冲区第一个字节，不取出 |
| `Serial.write(buf,len)` | `Serial.write(data, length)` | 输出原始二进制字节 |
| `Serial.end()` | `Serial.end()` | 关闭串口 |

简单接收示例

```
void setup(){
  Serial.begin(115200);
}
void loop(){
  if(Serial.available()>0){
    char ch = Serial.read();
    Serial.print("recv:");
    Serial.println(ch);
  }
}
```

## 6. 外部中断

表格

| 函数 | 语法 | 说明 |
| --- | --- | --- |
| `attachInterrupt()` | `attachInterrupt(pin, ISR_func, mode)` | 绑定外部中断 |
| `detachInterrupt()` | `detachInterrupt(pin)` | 解除中断绑定 |
| `noInterrupts()` | `noInterrupts()` | 全局关闭中断，保护临界代码 |
| `interrupts()` | `interrupts()` | 全局打开中断 |

触发模式：

- `RISING`：上升沿
- `FALLING`：下降沿
- `CHANGE`：电平发生变化

> 
> 中断回调函数注意事项
> 
> 
> 1. 中断函数不能使用 `delay()`、`Serial.print()`（不推荐）；
> 2. 代码尽量简短快速；
> 3. 被中断修改的全局变量必须加 `volatile`。

```
volatile uint32_t cnt = 0;
void isrFunc(){
  cnt++;
}
void setup(){
  Serial.begin(115200);
  attachInterrupt(10, isrFunc, FALLING);
}
void loop(){
  Serial.println(cnt);
  delay(500);
}
```

## 7. 数学工具函数

表格

| 函数 | 功能 |
| --- | --- |
| `abs(x)` | 取绝对值 |
| `constrain(x, min, max)` | 数值限幅 |
| `map(val, inMin, inMax, outMin, outMax)` | 线性映射转换 |
| `max(a,b)` | 返回较大值 |
| `min(a,b)` | 返回较小值 |
| `sqrt(x)` | 平方根 |
| `pow(base,exp)` | 幂运算 |
| `sin(rad)` / `cos(rad)` / `tan(rad)` | 三角函数，**参数为弧度，不是角度** |
| `round(x)` | 四舍五入 |
| `floor(x)` | 向下取整 |
| `ceil(x)` | 向上取整 |

示例

```
int v1 = constrain(130, 0, 100); //输出100
int v2 = map(512, 0, 1023, 0, 255);
```

## 8. 位操作

表格

| 函数 | 说明 |
| --- | --- |
| `bit(n)` | 返回第 n 位的掩码 `bit(0)=1` |
| `bitSet(x,n)` | 将 x 的第 n 位置 1 |
| `bitClear(x,n)` | 将 x 的第 n 位清 0 |
| `bitRead(x,n)` | 读取 x 第 n 位，返回 0 或 1 |
| `bitWrite(x,n,val)` | 设置 x 第 n 位为 0/1 |

```
int x = 0b00000101;
int b = bitRead(x, 2);
```

## 9. 随机数工具

表格

| 函数 | 说明 |
| --- | --- |
| `randomSeed(seed)` | 设置随机种子 |
| `random(max)` | 返回 `0 ~ max‑1` |
| `random(min,max)` | 返回 `min ~ max‑1` |

> 
> ESP32‑S3 推荐使用硬件真随机源：`randomSeed(esp_random());`

```
void setup(){
  Serial.begin(115200);
  randomSeed(esp_random());
}
void loop(){
  long r = random(1,100);
  Serial.println(r);
  delay(300);
}
```

## 10. String 字符串对象

> 
> 注意：高频拼接会产生内存碎片，ESP32 可用，尽量避免在 loop 高频大量拼接。

表格

| 方法 | 功能 |
| --- | --- |
| `.length()` | 获取字符串长度 |
| `.indexOf(str)` | 查找子串，找不到返回 `-1` |
| `.substring(start,end)` | 截取子串 |
| `.toInt()` | 字符串转 int 整数 |
| `.toFloat()` | 字符串转浮点数 |
| `.c_str()` | 返回 C 语言 const char * 指针，用于库函数传参 |
| `.concat(str)` | 字符串拼接 |
| `.operator+=` | `s+="abc"` 拼接简写 |
| `.equals(str)` | 判断字符串相等 |

```
String s = "ESP32‑S3";
Serial.println(s.length());
Serial.println(s.substring(0,5));
```

## 11. SPI 总线（ESP32‑S3）

> 
> ST7789 屏幕使用 SPI；ESP32‑S3 支持软件重映射 SPI 引脚。

头文件：`#include <SPI.h>`

表格

| 函数 | 说明 |
| --- | --- |
| `SPI.begin()` | 初始化硬件 SPI |
| `SPI.setFrequency(freq)` | 设置 SPI 时钟频率，单位 Hz |
| `SPI.transfer(val)` | 发送 1 字节，同时接收 1 字节 |
| `SPI.transfer(buf,size)` | 批量收发缓冲区 |
| `SPI.end()` | 关闭 SPI |

> 
> TFT_eSPI 库内部已经封装 SPI，一般不需要手动调用 SPI 底层。

## 12. I2C (TWI) 总线

头文件：`#include <Wire.h>`

表格

| 函数 | 说明 |
| --- | --- |
| `Wire.begin()` | I2C 初始化 |
| `Wire.beginTransmission(addr)` | 开始向指定从机地址发送 |
| `Wire.write(data)` | 写入字节 |
| `Wire.endTransmission()` | 结束传输 |
| `Wire.requestFrom(addr, len)` | 请求读取从机 len 个字节 |
| `Wire.available()` | 获取接收缓冲区字节数 |
| `Wire.read()` | 读取一个字节 |

## 13. ESP32‑S3 WiFi 基础

头文件：`#include <WiFi.h>`

表格

| 函数 | 说明 |
| --- | --- |
| `WiFi.begin(ssid,password)` | 连接 WiFi |
| `WiFi.status()` | 获取连接状态；返回 `WL_CONNECTED` 代表连接成功 |
| `WiFi.isConnected()` | 返回 bool，是否已联网 |
| `WiFi.disconnect()` | 断开 WiFi |
| `WiFi.reconnect()` | 尝试重连 |
| `WiFi.localIP()` | 获取本机 IP |
| `WiFi.SSID()` | 获取当前连接 wifi 名称 |

> 
> ⚠️禁止死循环 `while(!WiFi.isConnected());`，必须增加超时防止程序卡死白屏。

示例：

```
const char* ssid = "xxx";
const char* pwd = "xxx";
void setup(){
  Serial.begin(115200);
  WiFi.begin(ssid,pwd);
  int timeout = 0;
  while(WiFi.status() != WL_CONNECTED && timeout <20){
    delay(500);
    timeout++;
  }
}
```

## 14. ESP32‑S3 NTP 网络时间

头文件

```
#include <WiFiUdp.h>
#include <NTPClient.h>
```

表格

| 方法 | 说明 |
| --- | --- |
| `NTPClient.begin()` | 启动 NTP 客户端 |
| `NTPClient.update()` | 更新时间 |
| `NTPClient.getFormattedTime()` | 获取时分秒字符串 |
| `NTPClient.getDay()` | 获取星期 0‑6，0 = 周日 |
| `NTPClient.getHours()` / `getMinutes()` / `getSeconds()` | 获取时分秒 |
| `NTPClient.getYear()` / `getMonth()` / `getDayOfMonth()` | 获取公历年月日 |

> 
> 时区偏移：东八区 `8 * 3600`

```
WiFiUDP ntpUDP;
NTPClient timeClient(ntpUDP, "ntp.aliyun.com", 8*3600, 60000);
```

## 15. ESP32‑S3 HTTPClient HTTP 请求

头文件：`#include <HTTPClient.h>`

```
HTTPClient http;
http.begin("https://xxx");
int code = http.GET();
if(code>0){
  String payload = http.getString();
}
http.end();
```

表格

| 方法 | 说明 |
| --- | --- |
| `.begin(url)` | 设置请求地址 |
| `.GET()` | GET 请求，返回 http 状态码 |
| `.getString()` | 获取响应文本 |
| `.getPayloadLength()` | 返回数据长度 |
| `.end()` | 释放资源，必须调用 |

> 
> 注意：HTTPS 需要证书，简单项目优先使用 HTTP 接口。

## 16. ESP32‑S3 Flash 文件系统 SPIFFS / LittleFS

> 
> ESP32‑S3 推荐使用 **LittleFS**，SPIFFS 已逐步废弃。

头文件：`#include <LittleFS.h>`

表格

| 函数 | 说明 |
| --- | --- |
| `LittleFS.begin()` | 挂载文件系统 |
| `LittleFS.open(path,"r")` | 打开文件读 |
| `LittleFS.open(path,"w")` | 打开文件写 |
| `.read()` | 读取字节 |
| `.write()` | 写入字节 |
| `.close()` | 关闭文件 |
| `LittleFS.exists(path)` | 判断文件是否存在 |
| `LittleFS.remove(path)` | 删除文件 |

## 17. 常用系统常量

```
//电平
HIGH
LOW

//引脚模式
OUTPUT
INPUT
INPUT_PULLUP

//中断触发
RISING
FALLING
CHANGE

//WiFi状态
WL_CONNECTED
WL_DISCONNECTED

//内置LED
LED_BUILTIN
```

## 18. ESP32‑S3 开发板重点注意事项

1. **Strapping 启动引脚：GPIO0、GPIO45、GPIO46**，上电有特殊电平，尽量不要作为屏幕、LED 输出；
2. ADC：`analogRead()`返回 0‑4095，不是 Uno 的 0‑1023；ADC1 部分引脚与 SPI 冲突；
3. 无`analogWrite()`，PWM 全部使用`ledc`；
4. SPI、I2C 引脚可以软件映射，不必使用硬件默认引脚；
5. 老 ESP32 的 GPIO32‑34，ESP32‑S3 多数开发板没有引出，复制旧代码注意核对；
6. `millis()`变量必须使用`unsigned long`，不要用 int/long；
7. WiFi 连接一定要做超时，禁止死循环等待，否则屏幕项目会白屏卡死；
8. LittleFS 替代 SPIFFS，上传文件需要使用插件上传文件镜像。

## 附录 A：通用最小代码模板

```
#define LED_PIN 2

void setup() {
  Serial.begin(115200);
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_PIN, HIGH);
  delay(500);
  digitalWrite(LED_PIN, LOW);
  delay(500);
}
```

## 附录 B：常见踩坑清单

1. 编译报错：`no matching function` → 库版本升级，旧函数被废弃，查阅库 examples；
2. 屏幕白屏：TFT_eSPI 配置写在`User_Setup.h`，不要只在代码定义引脚；检查 BL 背光引脚；
3. millis 计时错乱：变量没有写`unsigned long`；
4. WiFi 卡死白屏：`while(!WiFi.begin())`死循环，缺少超时；
5. 中断变量没有加`volatile`，读取数值异常；
6. ESP32‑S3 直接复制 ESP32 老项目引脚，32‑34 不存在导致外设无响应。

---

> 作者: [Buray](http://www.buray.top)  
> URL: http://www.buray.top/posts/arduinoesp32s3manual/  

