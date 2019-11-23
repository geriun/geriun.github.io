# 管脚

|ACT|ACT|ACT|PIN|................   |PIN|ACT|ACT|ACT|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|   |TOUT|ADC0|A0|   |D0|GPIO16|USER|WAKE|
|   |   |RESVRVED|RSV|   |D1|GPIO5|   |   |
|   |   |RESVRVED|RSV|   |D2|GPIO4|   |   |
|   |SDD3|GPIO10|SD3|   |D3|GPIO0|FLASH|   |
|5V POWER|SDD2|GPIO9|SD2|   |D4|GPIO2|TXD1|   |
|3.3V|SDD1|MOSI|SD1|   |3V3|3.3V|   |   |
|GROUND|SDCCMD|CS|CMD|   |GND|GND|
|GPIO|SDD0|MISO|SD0|   |D5|GPIO14|   |HSCLK|
|SDIO|SDCLK|SCLK|CLK|   |D6|GPIO12|   |HMISO|
|UART|   |GND|GND|   |D7|GPIO13|RXD2|HMOSI|
|HSPI/SPI|   |3.3V|3V3|   |D8|GPIO15|TXD2|HCS|
|KEY||EN|EN||D9|GPIO3|RXD0|
|SYSTEM||RST|RST||D10|GPI01|TXD0|
|ADC||GND|GND||GND|GND|
|RESERVED||VINSV|VIN||3V3|3.3V|


# 代码示例

1.点亮板载LED(GPIO16)

```c
int led 16;
void setup(){
    pinMode(led,OUTPUT);
}

void loop(){
    digitalWrite(led,LOW);
}
```

2.设置WiFi(STA+AP模式)

```c
#include<ESP8266WiFi.h>
//AP Config
IPAddress APIP(192,168,10,1);
IPAddress APGATE(192,168,10,1);
IPAddress APMASK(225,225,225,0);
//STA Config
IPAddress STAIP(192,168,2,220);
IPAddress STAGATE(192,168,2,1);
IPAddress STAMASK(225,225,225,0);
//HostName
String hname ="ESP_Demo";

void setup() {
  //Setup COM
  Serial.begin(115200);
  Serial.println();
  Serial.println("Boot!");

  //Setup WiFiMode
  WiFi.mode(WIFI_AP_STA);
  Serial.print("Wifi Initing!Please wait...");
  Serial.println();

  //WiFi
  WiFi.begin("GCLD","501501501");
  WiFi.config(STAIP,STAGATE,STAMASK);

  //AP
  WiFi.softAPConfig(APIP,APGATE,APMASK);
  WiFi.softAP("ESP_Test","01020304");

  //Setup HostName
  WiFi.hostname(hname);
  
  //Print WiFi Config
  delay(5000);
  Serial.print("WifiStatusCode:");
  Serial.println(WiFi.status());
  Serial.print("STAIP:");
  Serial.println(WiFi.localIP());
  Serial.print("APIP:");
  Serial.println(WiFi.softAPIP());
}

void loop() {
  
}
```