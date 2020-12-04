---
keyword: [物联网, IoT, 物联网平台, 服务端订阅, 消息服务（MNS）队列]
---

# 服务端订阅（MNS）

本示例介绍如何配置服务端订阅，将产品下的设备状态变化消息推送到消息服务（MNS）队列中。服务器通过监听MNS队列接收设备状态变化消息。

-   开通阿里云产品：
    -   [物联网平台](https://www.aliyun.com/product/iot-devicemanagement)
    -   [消息服务](https://www.aliyun.com/product/mns)
-   安装Java开发环境Eclipse。

数据流转流程如下图所示。

![设备消息订阅](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6634749951/p48746.png)

## 配置服务端订阅

首先在物联网平台控制台创建MNS服务端订阅，选择要订阅的消息类型。

1.  登录[物联网平台控制台](https://iot.console.aliyun.com/)。

2.  在左侧导航栏，选择**设备管理** \> **产品**，再单击**创建产品**，创建一个气体监测仪产品。

3.  选择**设备** \> **添加设备**，在刚创建的气体监测仪产品下创建设备。

    设备证书信息将会用于设备端SDK开发配置。

4.  在左侧导航栏，选择**规则引擎** \> **服务端订阅**，再单击**创建订阅**，创建MNS服务端订阅。详细操作指导，请参见[使用MNS服务端订阅](/intl.zh-CN/消息通信/服务端订阅/使用MNS服务端订阅.md)。

    **说明：** 首次设置推送到MNS时，需单击提示中的**授权**，进入RAM控制台同意授权IoT访问MNS。

    本示例中，选择了**设备状态变化通知**，即该产品下所有设备的状态变化消息，都会被推送到MNS队列中。

    订阅成功后，物联网平台会在MNS中，自动创建一个接收物联网平台消息的队列。队列名称格式为：`aliyun-iot-${yourProductKey}`。您在配置MNS SDK监听消息时，需填入该队列名称。

    在订阅列表中，单击MNS右侧的图标，可查看MNS队列名称。

    ![设备消息订阅](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9254707061/p48775.png)


## 配置服务端MNS SDK接收消息

本示例使用MNS Java SDK Demo。

1.  访问[MNS Java SDK下载]()，下载sample包aliyun-sdk-mns-samples1.1.8，并解压缩。

2.  打开Eclipse，选择导入工程，导入aliyun-sdk-mns-samples文件夹。

3.  在计算机的本地目录C:\\Users\\$\{YourComputerUserName\}下，添加一个PROPERTIES文件.aliyun-mns，并在文件中输入MNS访问身份认证信息，格式如下：

    ```
    mns.accountendpoint=http://$your_accountId.mns.$your_regionId.aliyuncs.com
    mns.accesskeyid=$your_accesskeyid
    mns.accesskeysecret=$your_accesskeysecret
    ```

    |参数|说明|
    |--|:-|
    |accountendpoint|您的MNS服务Endpoint。请在[消息服务控制台](https://mns.console.aliyun.com/)，选择队列所在地域单击**获取Endpoint**查看。|
    |accesskeyid|您账号的AccessKey ID。 将光标定位到您的账号头像上，选择**accesskeys**，进入安全信息管理页，可创建或查看您的AccessKey。 |
    |accesskeysecret|您账号的AccessKey Secret。查看方法同上AccessKey ID。|

4.  在src\\main\\java\\com.aliyun.mns.sample.Queue目录下的ComsumerDemo文件中，配置物联网平台自动创建的消息服务队列名称。

    ```
        public static void main(String[] args) {
            CloudAccount account = new CloudAccount(
                    ServiceSettings.getMNSAccessKeyId(),
                    ServiceSettings.getMNSAccessKeySecret(),
                    ServiceSettings.getMNSAccountEndpoint());
            MNSClient client = account.getMNSClient(); //client初始化
    
            // 提取消息
            try{
                CloudQueue queue = client.getQueueRef("aliyun-iot-a1eN7La****");// 替换为物联网平台自动创建的队列
                for (int i = 0; i < 10; i++)
                {
                    Message popMsg = queue.popMessage(); //长轮询等待时间
                    if (popMsg != null){
                        System.out.println("message handle: " + popMsg.getReceiptHandle());
                        System.out.println("message body: " + popMsg.getMessageBodyAsString()); //获取原始消息
                        System.out.println("message id: " + popMsg.getMessageId());
                        System.out.println("message dequeue count:" + popMsg.getDequeueCount());
                        //<<to add your special logic.>>
    
                        //从队列中删除消息
                        queue.deleteMessage(popMsg.getReceiptHandle());
                        System.out.println("delete message successfully.\n");
                    }
                }
            }
    ```

5.  运行ComsumerDemo。


## 配置设备端SDK

1.  访问[下载设备端SDK](/intl.zh-CN/设备接入/下载设备端SDK.md)，选择Java SDK。

2.  在Java SDK 工程配置文档页底部，单击下载Java SDK Demo，然后解压缩。

3.  在Java开发工具中，导入工程，选择导入JavaLinkKitDemo文件夹。

4.  在device\_id文件中，填入设备证书信息。

    ![设备消息订阅](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6634749951/p48847.png)

5.  在src\\devicesdk\\demo目录下的MqttSample文件中，填入设备信息，将publish对应的Topic配置为您的设备Topic。

    ![设备消息订阅](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6634749951/p48855.png)

6.  运行设备连接Demo：MqttSample。


## 结果验证

设备端SDK运行后，设备上线消息通过服务端订阅发送到消息服务队列中。消息服务SDK从队列中接收消息，并同时将已接收的消息从队列中删除。

下图展示消息服务SDK接收并删除消息。

![设备消息订阅](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6634749951/p50785.png)

