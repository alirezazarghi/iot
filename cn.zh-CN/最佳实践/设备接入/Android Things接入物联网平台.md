---
keyword: [物联网, IoT, 物联网平台, Android Things, NXP Pico i.MX7D]
---

# Android Things接入物联网平台

本文档以室内空气检测项目为例，介绍如何将谷歌Android Things物联网硬件接入阿里云物联网平台。

## 硬件设备

-   项目设备列表

    下表中为室内空气检测所需的硬件设备：

    |设备|设备图片|备注|
    |--|----|--|
    |NXP Pico i.MX7D

开发板

|![Android Things](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7231649951/p7864.png)

|基于Android things系统1.0。

如需更多帮助，请参见NXP Pico i.MX7D I/O官网接口文档：[开发者指南](https://developer.android.com/things/hardware/imx7d-pico-io)。

**说明：** 该硬件可以用树莓派替代。请参见[远程控制树莓派服务器](/cn.zh-CN/最佳实践/消息通信/远程控制树莓派服务器.md)。 |
    |DHT12

温湿度传感器

|![Android Things](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7231649951/p7865.png)

|采用I2C数据通信方式。|
    |ZE08-CH2O

甲醛检测传感器

|![Android Things](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7231649951/p7866.png)

|采用UART数据通信方式。|

-   设备接线示意图

    ![Android Things](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7231649951/p7908.png)

    -   将温湿度传感器（DHT12）的时钟信号线引脚SCL和数据线引脚SDA分别与开发板的I2C总线的SCL和SDA引脚相接。
    -   将甲醛检测传感器（ZE08-CH2O）的发送数据引脚TXD与开发板的接收数据引脚RXD相接；将ZE08-CH2O的接收数据引脚RXD与开发板的发送数据引脚TXD相接。

## 创建阿里云物联网平台产品和设备

如果您还未开通阿里云物联网平台服务，请先了解、开通[物联网平台](https://www.aliyun.com/product/iot)。

1.  登录[阿里云物联网平台控制台](https://iot.console.aliyun.com)。

2.  创建产品。

    1.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

        ![实例概览](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8727475061/p174584.png)

    2.  在左侧导航栏，单击**设备管理**，选择**产品**

    3.  在**产品**页，单击**创建产品**进入产品创建流程。详情操作说明，请参见[创建产品](/cn.zh-CN/设备接入/创建产品.md)。

3.  定义物模型。

    1.  在产品创建成功页面，单击**前往定义物模型**。

    2.  在产品详情页的功能定义页签下，单击**编辑草稿** \> **添加自定义功能**。

    3.  添加以下属性。

        |属性名称|标识符|数据类型|取值范围|描述|
        |:---|:--|:---|:---|:-|
        |温度|temperature|float|-50~100|DHT12温湿度传感器采集的温度数据。|
        |湿度|humidity|float|0~100|DHT12温湿度传感器采集的湿度数据。|
        |甲醛浓度|ch2o|double|0~3|ZE08甲醛检测传感器采集的甲醛浓度。|

    4.  单击**发布上线**发布物模型。

4.  创建设备。

    在设备页面，单击**添加设备**，在产品下创建设备。详情操作说明，请参见[单个创建设备](/cn.zh-CN/设备接入/创建设备/单个创建设备.md)。


## 开发Android things设备

1.  使用Android Studio创建Android things工程，添加网络权限。

    ```
    <uses-permission android:name="android.permission.INTERNET" />
    ```

2.  在gradle中添加`eclipse.paho.mqtt`。

    ```
    implementation 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.2.0'
    ```

3.  配置通过I2C读取温湿度传感器DHT12的数据。

    ```
    private void readDataFromI2C() {
    
            try {
    
                byte[] data = new byte[5];
                i2cDevice.readRegBuffer(0x00, data, data.length);
    
                // check data
                if ((data[0] + data[1] + data[2] + data[3]) % 256 != data[4]) {
                    humidity = temperature = 0;
                    return;
                }
                // humidity data
                humidity = Double.valueOf(String.valueOf(data[0]) + "." + String.valueOf(data[1]));
                Log.d(TAG, "humidity: " + humidity);
                // temperature data
                if (data[3] < 128) {
                    temperature = Double.valueOf(String.valueOf(data[2]) + "." + String.valueOf(data[3]));
                } else {
                    temperature = Double.valueOf("-" + String.valueOf(data[2]) + "." + String.valueOf(data[3] - 128));
                }
    
                Log.d(TAG, "temperature: " + temperature);
    
            } catch (IOException e) {
                Log.e(TAG, "readDataFromI2C error " + e.getMessage(), e);
            }
    
        }
    ```

4.  配置通过UART获取甲醛检测传感器Ze08-CH2O的数据。

    ```
    try {
                    // data buffer
                    byte[] buffer = new byte[9];
    
                    while (uartDevice.read(buffer, buffer.length) > 0) {
    
                        if (checkSum(buffer)) {
                            ppbCh2o = buffer[4] * 256 + buffer[5];
                            ch2o = ppbCh2o / 66.64 * 0.08;
                        } else {
                            ch2o = ppbCh2o = 0;
                        }
                        Log.d(TAG, "ch2o: " + ch2o);
                    }
    
                } catch (IOException e) {
                    Log.e(TAG, "Ze08CH2O read data error " + e.getMessage(), e);
                }
    ```

5.  创建阿里云物联网平台与设备端的连接，上报数据。

    ```
    /*
    payload格式
    {
      "id": 123243,
      "params": {
        "temperature": 25.6,
        "humidity": 60.3,
        "ch2o": 0.048
      },
      "method": "thing.event.property.post"
    }
    */
    MqttMessage message = new MqttMessage(payload.getBytes("utf-8"));
    message.setQos(1);
    
    String pubTopYourPc = "/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post";
    
    mqttClient.publish(pubTopic, message);
    ```


请访问[GitHub阿里云IoT](https://github.com/iot-blog/aliyun-iot-android-things-nxp)，下载完整的示例工程代码。

## 查看实时数据

设备启动后，登录[物联网平台控制台](http://iot.console.aliyun.com/)，在对应实例下，找到设备详情页，在运行状态页签下，查看设备当前的实时属性数据。

![Android Things](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7231649951/p8025.png)

