---
keyword: [物联网, IoT, 物联网平台, M2M, 规则引擎, 数据流转, 设备通信]
---

# 基于规则引擎的M2M设备间通信

本文以智能灯和手机App连接为例，基于物联网平台的规则引擎数据流转功能，构建一个M2M设备间通信架构。

使用手机App控制智能灯的流程：

![M2M设备间通信](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4531649951/p4208.png)

1.  在[物联网平台控制台](https://iot.console.aliyun.com/)，为智能灯设备创建产品和设备，定义功能等。请参见文档[创建产品](/cn.zh-CN/设备接入/创建产品.md)、[批量创建设备](/cn.zh-CN/设备接入/创建设备/批量创建设备.md)、[单个添加物模型](/cn.zh-CN/设备管理/物模型/单个添加物模型.md)。

    本示例中，智能灯的ProductKey为al123456789；DeviceName为light。

2.  开发智能灯设备端。

    本示例中，设备与物联网平台间的通信协议为MQTT。

    设备端SDK开发详情，请参见[设备端Link SDK文档](https://help.aliyun.com/product/93051.html)。

3.  在物联网平台，为手机App注册产品和设备。

    本示例中，手机App的ProductKey为al987654321；DeviceName为ControlApp。

    当手机App用户注册登录时，您的服务器将App的设备信息发送给手机App，让手机App可以作为一个设备连接到物联网平台。

4.  开发手机App。

    本示例中，手机App与物联网平台间的通信协议为HTTPS。

    手机App发送的智能灯控制指令payload数据格式如下：

    ```
    {
        "TargetDevice": "light", 
        "Switch": "off", 
        "Timestamp": 1557750407000
    }
    ```

5.  开发智能灯设备端，实现智能灯设备连接物联网平台，接收并执行指令等功能。

6.  设置规则引擎数据流转规则，将手机App发布的指令流转到智能灯的Topic中。

    1.  登录[物联网平台控制台](https://iot.console.aliyun.com/)，在对应实例下，选择**规则引擎** \> **云产品流转**。

    2.  单击**创建规则**，创建一个数据流转规则

    3.  编写处理转发消息内容的SQL。该SQL将从手机App设备的Topic消息中，筛选出要发送给智能灯的消息字段。

        本示例中，SQL将筛选出消息中的目标设备的名称TargetDevice，消息时间戳Timestamp和Switch三个字段的值。

        ![M2M设备间通信](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4531649951/p47216.png)

    4.  设置转发消息目的地。将智能灯设备具有订阅权限的Topic作为接收手机App指令的Topic。

        **说明：**

        -   选择**发布到另一个Topic中**。
        -   指定设备时，需使用转义符`${TargetDevice}`通配所有目标智能灯。如本示例中`${TargetDevice}`会被转义成智能灯设备名称`light`。
        ![M2M设备间通信](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4531649951/p47218.png)

7.  手机App用户通过扫码将App与智能灯绑定。

    当App向您的服务器发送绑定某设备的请求后，您的服务器将返回绑定成功的智能灯设备名称deviceName。本示例中，智能灯设备名称为light。

8.  手机App用户通过App发送控制指令。

    1.  手机App向物联网平台中的Topic发送指令，如本示例中，App对应的发送指令Topic：`/al987654321/ControlApp/user/command`。

    2.  物联网平台再根据您定义的数据流转规则，将指令信息发送给智能灯的Topic，如本示例中定义的Topic为：`/al123456789/light/user/set`。

    3.  智能灯接收到指令后，执行相关操作。

    **说明：** 手机App也可以向您的服务器发送解绑设备的请求。解绑后，该手机App将不再控制该智能灯。


