---
keyword: [物联网, IoT, 物联网平台, 规则引擎, 数据流转, 消息队列RocketMQ, 服务器]
---

# 设备消息通过RocketMQ流转到服务器

物联网平台将设备上报的数据流转至消息队列RocketMQ的Topic中，然后，RocketMQ再将数据流转到您的服务器。

-   已注册阿里云账号。
-   已开通阿里云物联网平台服务。

    如未开通，请登录[物联网平台产品页](https://www.aliyun.com/product/iot?spm=5176.8142029.388261.381.a7236d3eaQEJCn)，单击**立即开通**，根据页面提示，开通服务。

-   已开通消息队列RocketMQ服务。

    如未开通，请登录[消息队列RocketMQ产品页](https://www.aliyun.com/product/rocketmq)，开通服务。


建议架构：

![消息流转MQ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6531649951/p33636.png)

方案优势：

通过消息队列RocketMQ消峰去谷，缓冲消息，减轻服务器同时接收大量设备消息的压力。

1.  登录[物联网控制台](https://iot.console.aliyun.com/)，创建产品和设备。

    1.  在实例概览页，找到对应的实例，单击实例进入实例详情页。

        ![实例概览](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9275903061/p174584.png)

    2.  左侧菜单栏选择**设备管理** \> **产品**，单击**创建产品**，创建产品。本示例中，创建了名称为MQ\_test的产品，且节点类型选择为设备。

    3.  在产品详情页面，自定义Topic类，用于设备上报数据。本示例中，定义的Topic类：`/{YourProductKey}/${YourDeviceName}/user/data`。

    4.  选择**设备管理** \> **设备** \> **添加设备**，创建设备。本示例中，创建了一个名称为MQdevice的设备。

2.  在消息队列RocketMQ控制台，创建Topic和消费者。

    1.  登录[消息队列RocketMQ控制台](https://ons.console.aliyun.com/)。

    2.  创建一个实例。

    3.  创建Topic。消息类型选择**普通消息**。

        ![物联网设备消息](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6531649951/p4249.png)

    4.  创建Group ID。

        ![物联网设备消息](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6531649951/p37803.png)

    5.  创建消息消费者。在服务器SDK中运行如下示例代码，然后，在RocketMQ控制台查看消费者状态，消费者是否处于在线状态，订阅关系是否一致。

        ```
        import com.aliyun.openservices.ons.api.Action;
        import com.aliyun.openservices.ons.api.ConsumeContext;
        import com.aliyun.openservices.ons.api.Consumer;
        import com.aliyun.openservices.ons.api.Message;
        import com.aliyun.openservices.ons.api.MessageListener;
        import com.aliyun.openservices.ons.api.ONSFactory;
        import com.aliyun.openservices.ons.api.PropertyKeyConst;
        import java.util.Properties;
        public class ConsumerTest {
            public static void main(String[] args) {
                Properties properties = new Properties();
                // 您在控制台创建的 Group ID
                properties.put(PropertyKeyConst.GROUP_ID, "XXX");
                // AccessKey 阿里云身份验证，在阿里云服务器管理控制台创建
                properties.put(PropertyKeyConst.AccessKey, "${AccessKey}");
                // SecretKey 阿里云身份验证，在阿里云服务器管理控制台创建
                properties.put(PropertyKeyConst.SecretKey, "${SecretKey}");
                // 设置 TCP 接入域名，到控制台的实例基本信息中查看
                properties.put(PropertyKeyConst.NAMESRV_ADDR,
                    "XXX");
                // 集群订阅方式 (默认)
                // properties.put(PropertyKeyConst.MessageModel, PropertyValueConst.CLUSTERING);
                // 广播订阅方式
                // properties.put(PropertyKeyConst.MessageModel, PropertyValueConst.BROADCASTING);
                Consumer consumer = ONSFactory.createConsumer(properties);
                consumer.subscribe("iot_to_mq", "*", new MessageListener() { //订阅多个 Tag
                    public Action consume(Message message, ConsumeContext context) {
                        System.out.println("Receive: " + message);
                        return Action.CommitMessage;
                    }
                });
                consumer.start();
                System.out.println("Consumer Started");
            }
        }
        ```

        **说明：**

        -   如何创建AccessKey和SecretKey，请参见[创建AccessKey](https://help.aliyun.com/document_detail/53045.html)。
        -   RocketMQ详细操作指导，请参考[消息队列RocketMQ文档](https://help.aliyun.com/document_detail/34411.html)
3.  登录[物联网平台控制台](http://iot.console.aliyun.com/)，在对应实例下，设置数据流转规则，将设备上报的数据转发至消息队列（RocketMQ）。

    1.  单击**规则引擎** \> **云产品流转**，再单击**创建规则**来创建一条数据流转规则。数据格式选择为JSON。

    2.  设置数据处理SQL。

        ![物联网设备消息](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8217549951/p37804.png)

    3.  设置数据转发目的地。

        ![物联网设备消息](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/8217549951/p37805.png)

    4.  启动规则。

        规则启动后，物联网平台会将规则SQL中定义的设备上报消息转发至消息队列（RocketMQ）的Topic中。

4.  使用Java SDK模拟设备，上报消息。

    1.  下载[Java SDK Demo](http://gaic.alicdn.com/ztms/java-linkkit-demo-v0130/JavaLinkKitDemo.zip)。

    2.  输入MQdevice的设备证书信息，包括ProductKey、DeviceName和DeviceSecret。

    3.  修改MQTT Topic为设备上报数据的Topic。本示例中，使用的Topic是`/{YourProductKey}/${YourDeviceName}/user/data`。

    4.  启动设备。

    登录[物联网平台控制台](http://iot.console.aliyun.com/)，在对应的实例下，选择**监控运维** \> **日志服务**，查看该设备的日志信息，发现设备数据成功转发至RocketMQ。

    ![物联网设备消息](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6531649951/p40673.png)

5.  在RocketMQ控制台查看消息。

    1.  在本地运行订阅消息队列RocketMQ资源的代码。

        ![物联网设备消息](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6531649951/p4274.png)

    2.  在消息队列RocketMQ控制台，消息查询页面，按Topic或者Message ID查询消息，验证消息是否成功流转至消息队列RocketMQ中。

        RocketMQ接收到的消息类型：

        ```
        {"deviceName()":"MQdevice"}
        ```


