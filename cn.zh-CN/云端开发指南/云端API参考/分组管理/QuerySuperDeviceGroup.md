# QuerySuperDeviceGroup

调用该接口根据子分组ID查询父分组信息。

## 限制说明

单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

**说明：** RAM用户共享阿里云账号配额。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Iot&api=QuerySuperDeviceGroup&type=RPC&version=2018-01-20)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|QuerySuperDeviceGroup|系统规定参数。取值：QuerySuperDeviceGroup。 |
|GroupId|String|是|DMoI2Kby5m62\*\*\*\*|子分组ID，分组的全局唯一标识符。 |
|IotInstanceId|String|否|iot\_instc\_pu\*\*\*\*\_c\*-v64\*\*\*\*\*\*\*\*|实例ID。公共实例不传此参数，企业版实例需传入。 |

调用API时，除了本文介绍的该API的特有请求参数，还需传入公共请求参数。公共请求参数说明，请参见[公共参数文档](~~30561~~)。

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|String|iot.system.SystemException|调用失败时，返回的错误码。更多信息，请参见[错误码](~~87387~~)。 |
|Data|Array of GroupInfo| |调用成功时，返回的父分组信息数据。请参见以下**GroupInfo**。 |
|GroupInfo| | | |
|GroupDesc|String|A test|父分组描述。 |
|GroupId|String|tDQvBJqbUyHskDse|子分组所属的父分组ID。 |
|GroupName|String|IOTTEST|子分组所属的父分组名称。 |
|ErrorMessage|String|系统异常|调用失败时，返回的出错信息。 |
|RequestId|String|7411716B-A488-4EEB-9AA0-6DB05AD2491F|阿里云为该请求生成的唯一标识符。 |
|Success|Boolean|true|是否调用成功。

 -   **true**：调用成功。
-   **false**：调用失败。 |

## 示例

请求示例

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QuerySuperDeviceGroup
&GroupId=DMoI2Kby5m62****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<QuerySuperDeviceGroupResponse>
      <Data>
            <GroupName>IOTTEST</GroupName>
            <GroupId>tDQvBJqbUyHskDse</GroupId>
            <GroupDesc>A test.</GroupDesc>
      </Data>
      <RequestId>7411716B-A488-4EEB-9AA0-6DB05AD2491F</RequestId>
      <Success>true</Success>
</QuerySuperDeviceGroupResponse>
```

`JSON` 格式

```
{
    "Data":{
        "GroupName":"IOTTEST",
        "GroupId":"tDQvBJqbUyHskDse",
    "GroupDesc":"A test."
    },
    "RequestId":"7411716B-A488-4EEB-9AA0-6DB05AD2491F",
    "Success":true
}
```

