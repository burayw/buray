# TFT_eSPI ST7789(2.4寸) ESP32‑S3 使用手册


# TFT_eSPI ST7789 2.4寸屏 ESP32‑S3 使用手册
> 针对ESP32‑S3 + ST7789 SPI屏幕，解决白屏、编译报错、引脚配置问题。

## 目录
- [TFT\_eSPI ST7789 2.4寸屏 ESP32‑S3 使用手册](#tft_espi-st7789-24寸屏-esp32s3-使用手册)
  - [目录](#目录)
  - [1.硬件接线说明](#1硬件接线说明)
  - [2.User\_Setup.h关键配置](#2user_setuph关键配置)
  - [3. 屏幕方向 setRotation](#3-屏幕方向-setrotation)
  - [4. 基础绘图 API](#4-基础绘图-api)
  - [5. 文字显示 API](#5-文字显示-api)
  - [6. 颜色 565 转换](#6-颜色-565-转换)
  - [7. 最小可运行测试代码](#7-最小可运行测试代码)
  - [8. 常见编译报错处理](#8-常见编译报错处理)
    - [报错 1：`no matching function for call to 'TFT_eSPI::begin(...)'`](#报错-1no-matching-function-for-call-to-tft_espibegin)
    - [报错 2：`drawArc no matching function`](#报错-2drawarc-no-matching-function)
    - [报错 3：缩小转换 narrowing conversion uint16\_t](#报错-3缩小转换-narrowing-conversion-uint16_t)
  - [9. 硬件故障排查（白屏、花屏、无显示）](#9-硬件故障排查白屏花屏无显示)
    - [现象：背光亮，但是全黑无任何图像](#现象背光亮但是全黑无任何图像)
    - [现象：花屏乱码](#现象花屏乱码)
  - [附录 A：常用预定义颜色表](#附录-a常用预定义颜色表)
  - [附录 B：屏幕项目最佳实践](#附录-b屏幕项目最佳实践)

## 1.硬件接线说明
ST7789 SPI屏，ESP32‑S3，**S3没有GPIO32/33/34，避开strapping引脚GPIO0,GPIO45,GPIO46**

| ST7789屏引脚 | ESP32‑S3推荐GPIO |
|---|---|
| VCC | 3.3V **禁止接5V** |
| GND | GND |
| SCLK(SCL) | GPIO12 |
| MOSI(SDA) | GPIO11 |
| CS | GPIO10 |
| DC(A0) | GPIO9 |
| RST(RES) | GPIO8 |
| BL(背光) | GPIO7 |

> ⚠️注意：
> 1.屏幕必须3.3V供电，很多ST7789模块不支持5V输入，接5V直接烧毁；
> 2.MISO引脚ST7789不需要，不用接线；
> 3.BL背光：部分模块是高电平点亮，少数低电平点亮，白屏时优先测试背光。

## 2.User_Setup.h关键配置
> TFT_eSPI引脚**不能只写在代码里，必须修改库内User_Setup.h**，这是绝大多数白屏根源。

打开库文件：`TFT_eSPI/User_Setup.h`
```cpp
#define ST7789_DRIVER

#define TFT_WIDTH  240
#define TFT_HEIGHT 320

#define TFT_SPI_PORT 1

#define TFT_SCLK  12
#define TFT_MOSI  11
#define TFT_CS    10
#define TFT_DC     9
#define TFT_RST    8

// 如果RST接到开发板RST，设置-1
//#define TFT_RST  -1

#define TFT_BACKLIGHT_ON HIGH

#define SPI_FREQUENCY  40000000
#define SPI_READ_FREQUENCY  20000000
```

修改完成后保存，重启 ArduinoIDE。

## 3. 屏幕方向 setRotation

```
tft.setRotation(0);
```

- 0：竖屏 240 (w) ×320 (h)
- 1：横屏 320 (w) ×240 (h)
- 2：竖屏倒置
- 3：横屏倒置

> 
> 修改 rotation 之后，屏幕坐标会自动切换，不需要手动交换宽高。

## 4. 基础绘图 API

```
tft.fillScreen(color);           //全屏填充颜色
tft.drawPixel(x,y,color);        //画点
tft.drawLine(x1,y1,x2,y2,color); //画线
tft.drawRect(x,y,w,h,color);     //空心矩形
tft.fillRect(x,y,w,h,color);     //实心矩形
tft.drawCircle(x,y,r,color);     //空心圆
tft.fillCircle(x,y,r,color);     //实心圆
tft.drawRoundRect(x,y,w,h,r,color);  //圆角矩形空心
tft.fillRoundRect(x,y,w,h,r,color);  //圆角矩形实心
```

## 5. 文字显示 API

```
tft.setTextColor(fgColor,bgColor); //文字前景色，背景色；背景写TFT_BLACK透明
tft.setTextSize(size);             //字号 1~7放大倍数
tft.setCursor(x,y);                //文字光标坐标
tft.println("hello");
tft.print("test");
```

示例：

```
tft.setTextColor(TFT_WHITE,TFT_BLACK);
tft.setTextSize(2);
tft.setCursor(10,10);
tft.println("ESP32‑S3 ST7789");
```

## 6. 颜色 565 转换

TFT 屏幕使用 RGB565 16 位颜色

```
uint16_t c = tft.color565(R,G,B);
//R/G/B 范围0‑255
```

## 7. 最小可运行测试代码

```
#include <TFT_eSPI.h>
TFT_eSPI tft = TFT_eSPI();

#define BL_PIN 7

void setup(void)
{
  Serial.begin(115200);
  pinMode(BL_PIN,OUTPUT);
  digitalWrite(BL_PIN,HIGH);

  tft.init();
  tft.setRotation(0);
  tft.fillScreen(TFT_BLACK);

  tft.setTextColor(TFT_WHITE,TFT_BLACK);
  tft.setTextSize(2);
  tft.setCursor(20,40);
  tft.println("ST7789 OK");

  tft.drawCircle(120,160,50,TFT_RED);
}

void loop(void)
{

}
```

## 8. 常见编译报错处理

### 报错 1：`no matching function for call to 'TFT_eSPI::begin(...)'`

> 
> 新版 TFT_eSPI 不再支持在`.begin()`传入引脚；引脚全部交由 User_Setup.h 管理，代码只写`tft.init()`。

### 报错 2：`drawArc no matching function`

> 
> 低版本 TFT_eSPI 没有 drawArc 函数，不要使用该接口，改用圆 + 线段实现圆弧。

### 报错 3：缩小转换 narrowing conversion uint16_t

> 
> 数组字面量数值超出 uint16 范围，把数组类型改为`uint32_t`。

## 9. 硬件故障排查（白屏、花屏、无显示）

1. **背光是否点亮**：测量 BL 引脚电平；部分屏幕 BL 低电平点亮，调换 HIGH/LOW 测试。
2. **供电 3.3V！不要接 5V**，很多 ST7789 模块 5V 会白屏损坏。
3. User_Setup.h 引脚必须和实际接线一一对应，**只在代码 define 引脚无效**。
4. RST 引脚必须接线，或者 User_Setup 设置 TFT_RST=-1 接系统复位。
5. SPI 频率降低，把`SPI_FREQUENCY`改为 20000000 测试，排除走线干扰。
6. 避开 strapping 引脚 GPIO0、GPIO45、GPIO46，不要用作屏幕信号线。
7. S3 没有 GPIO32/33/34，老 ESP32 代码直接复制过来会完全无输出。
8. 接线接触不良，杜邦线换一批。

### 现象：背光亮，但是全黑无任何图像

- DC、CS 引脚接错是最高概率原因，核对 User_Setup 和硬件。

### 现象：花屏乱码

- SCLK/MOSI 接反；SPI 频率过高；接线接触不良。

## 附录 A：常用预定义颜色表

```
TFT_BLACK       0x0000
TFT_NAVY        0x000F
TFT_DARKGREEN   0x03E0
TFT_DARKCYAN    0x03EF
TFT_MAROON      0x7800
TFT_PURPLE      0x780F
TFT_OLIVE       0x7BE0
TFT_LIGHTGREY   0xC618
TFT_DARKGREY    0x7BEF
TFT_BLUE        0x001F
TFT_GREEN       0x07E0
TFT_CYAN        0x07FF
TFT_RED         0xF800
TFT_MAGENTA     0xF81F
TFT_YELLOW      0xFFE0
TFT_WHITE       0xFFFF
```

## 附录 B：屏幕项目最佳实践

1. WiFi 联网必须增加超时，禁止死循环等待 WiFi，否则屏幕直接卡死白屏。
2. loop 中不要频繁调用 fillScreen 全屏刷新，会闪烁，尽量局部 fillRect 刷新。
3. 农历大数组注意类型，防止 uint16 溢出编译报错。
4. 不要在中断回调里面执行屏幕绘图，会 SPI 冲突死机。
5. 开发阶段优先跑最小测试代码，确认屏幕点亮，再叠加 WiFi、NTP、农历业务。

---

> 作者: [Buray](http://www.buray.top)  
> URL: http://www.buray.top/posts/tftespimanual-st7789/  

