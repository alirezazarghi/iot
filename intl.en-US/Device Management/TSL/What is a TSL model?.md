---
keyword: [Internet of Things, IoT, IoT Platform, TSL model, TSL, feature definition, property, event, service]
---

# What is a TSL model?

A Thing Specification Language \(TSL\) model digitizes a physical entity and builds a data model for the entity on IoT Platform. A TSL model is used to describe the features of an entity. This article introduces the concepts of a TSL model and describes how to use a TSL model.

A TSL model is a JSON-formatted file. A TSL model digitizes a physical entity, such as a sensor, vehicle-mounted device, building, or factory. A TSL model includes the properties, services, and events of an entity. A TSL model describes what the entity is, what the entity can do, and what information the entity can provide. After you define the properties, services, and events of a product, the features of the product are defined.

Product features are classified into the following three types: property, service, and event.

|Feature type|Description|
|:-----------|:----------|
|Property|Defines a type of data that devices can collect, for example, the temperature data that is collected by an environmental sensor. Properties support the GET and SET request methods. Application systems can initiate requests to obtain and set properties.|
|Service|Defines a device capability or method that can be invoked by the required applications. You can set input and output parameters. Compared with properties, services can be invoked by using a command to implement complex business logic. For example, you can invoke a service to perform a specific task.|
|Event|Defines a device runtime event. Each event contains notifications that require further actions or responses. You can set multiple output parameters for an event. For example, an event may be a notification for a completed task, a system failure, or a temperature alert. You can publish or subscribe to events.|

## Format of a TSL model

On the Define Feature tab of the Product Details page, you can click **TSL Model** to view the TSL model in the JSON format.

Structure of a TSL model:

**Note:** To demonstrate the full structure of the TSL model, the following example includes all parameters. However, the parameters may vary in actual scenarios. The string that follows each parameter is the description of the parameter. For the application scenarios of each parameter, see the parameter description.

```
{
    "schema": "The URL that you can visit to view the TSL model.",
    "profile": {
        productKey: "The product key of the product."
    },
    "properties": [
        {
            "identifier": "The ID of the property. The ID must be unique for a product.",
            "name": "The property name.",
            "accessMode": "The level of access to the property. Valid values: r and w, where r refers to read-only and rw refers to read and write.",
            "required": "Specifies whether this property is required for a standard feature.",
            "dataType": {
                "type": "The data type of the property. Valid values: int, float, double, text, date (UTC timestamp in the string format. Unit: milliseconds), bool (0 or 1), enum (integers that are defined in the same way as the data of the bool type), struct (a struct that can include the previous seven data types. "specs": [{}] is used to describe the included objects), and array (an array that can include the int, double, float, text, and struct data types).",
                "specs": {
                    "min": "The minimum value. This parameter is available only for properties of the int, float, and double data types.",
                    "max": "The maximum value. This parameter is available only for properties of the int, float, and double data types.",
                    "unit": "Optional. The unit of the property. This parameter is available only for properties of the int, float, and double data types.",
                    "unitName": "Optional. The name of the unit. This parameter is available only for properties of the int, float, and double data types.",
                    "size": "The number of elements in the array. Maximum value: 512. This parameter is available only for properties of the array data type.",
                    "step": "The step size. This parameter is unavailable for properties of the text or enum data type.",
                    "length": "The length of the data. Maximum value: 10240. This parameter is available only for properties of the text data type.",
                    "0": "The value of 0. This parameter is available only for properties of the bool data type.",
                    "1": "The value of 1. This parameter is available only for properties of the bool data type.",
                    "item": {
                        "type": "The type of the array element. This parameter is available only for properties of the array data type."
                    }
                }
            }
        }
    ],
    "events": [
        {
            "identifier": "The ID of each event. The ID must be unique for a product. The post event is pre-defined and used to report properties.",
            "name": "The name of the event.",
            "desc": "The description of the event.",
            "type": "The type of the event. Valid values: info, alert, and error.",
            "required": "Specifies whether this event is required for a standard feature.",
            "outputData": [
                {
                    "identifier": "The ID of each output parameter. The ID must be unique for a product.",
                    "name": "The name of the output parameter.",
                    "dataType": {
                        "type": "The data type of the output parameter. Valid values: int, float, double, text, date (UTC timestamp in the string format. Unit: milliseconds), bool (0 or 1), enum (integers that are defined in the same way as the data of the bool type), struct (a struct that can include the previous seven data types. "specs": [{}] is used to describe the included objects), and array (an array that can include the int, double, float, text, and struct data types).",
                        "specs": {
                            "min": "The minimum value. This parameter is available only for output parameters of the int, float, double types.",
                            "max": "The maximum value. This parameter is available only for output parameters of the int, float, and double types.",
                            "unit": "Optional. The unit of the property. This parameter is available only for output parameters of the int, float, and double data types.",
                            "unitName": "Optional. The name of the unit. This parameter is available only for output parameters of the int, float, and double data types.",
                            "size": "The number of elements in the array. Maximum value: 512. This parameter is available only for output parameters of the array data type.",
                            "step": "The step size. This parameter is unavailable for output parameters of the text or enum data type.",
                            "length": "The length of the data. Maximum value: 10240. This parameter is available only for output parameters of the text data type.",
                            "0": "The value of 0. This parameter is available only for output parameters of the bool data type.",
                            "1": "The value of 1. This parameter is available only for output parameters of the bool data type.",
                            "item": {
                                "type": "The type of the array element. This parameter is available only for output parameters of the array data type."
                            }
                        }
                    }
                }
            ],
            "method": "The name of a method that can access the event. The name is generated based on the value of the identifier parameter."
        }
    ],
    "services": [
        {
            "identifier": "The ID of the service. The ID must be unique for a product. The set and get services are pre-defined based on the value of the accessMode parameter that is specified in the properties.",
            "name": "The name of the service.",
            "desc": "The description of the service.",
            "required": "Specifies whether this service is required for a standard feature.",
            "callType": "The call type. Valid values: async and sync, where async refers to an asynchronous call and sync refers to a synchronous call.",
            "inputData": [
                {
                    "identifier": "The ID of the input parameter. The ID must be unique for a product.",
                    "name": "The name of the input parameter.",
                    "dataType": {
                        "type": "The data type of the input parameter. Valid values: int, float, double, text, date (UTC timestamp in the string format. Unit: milliseconds), bool (0 or 1), enum (integers that are defined in the same way as the data of the bool type), struct (a struct that can include the previous seven data types. "specs": [{}] is used to describe the included objects), and array (an array that can include the int, double, float, text, and struct data types).",
                        "specs": {
                            "min": "The minimum value. This parameter is available only for input parameters of the int, float, double types.",
                            "max": "The maximum value. This parameter is available only for input parameters of the int, float, and double types.",
                            "unit": "Optional. The unit of the property. This parameter is available only for input parameters of the int, float, and double data types.",
                            "unitName": "Optional. The name of the unit. This parameter is available only for input parameters of the int, float, and double data types.",
                            "size": "The number of elements in the array. Maximum value: 512. This parameter is available only for input parameters of the array data type.",
                            "step": "The step size. This parameter is unavailable for input parameters of the text or enum data type.",
                            "length": "The length of the data. Maximum value: 10240. This parameter is available only for input parameters of the text data type.",
                            "0": "The value of 0. This parameter is available only for input parameters of the bool data type.",
                            "1": "The value of 1. This parameter is available only for input parameters of the bool data type.",
                            "item": {
                                "type": "The type of the array element. This parameter is available only for input parameters of the array data type."
                                }
                            }
                        }
                    }
                }
            ],
            "outputData": [
                {
                    "identifier": "The ID of the output parameter.",
                    "name": "The name of the output parameter.",
                    "dataType": {
                        "type": "The data type of the output parameter. Valid values: int, float, double, text, date (UTC timestamp in the string format. Unit: milliseconds), bool (0 or 1), enum (integers that are defined in the same way as the data of the bool type), struct (a struct that can include the previous seven data types. "specs": [{}] is used to describe the included objects), and array (an array that can include the int, double, float, text, and struct data types).",
                        "specs": {
                            "min": "The minimum value. This parameter is available only for output parameters of the int, float, double data types.",
                            "max": "The maximum value. This parameter is available only for output parameters of the int, float, and double data types.",
                            "unit": "Optional. The unit of the property. This parameter is available only for output parameters of the int, float, and double data types.",
                            "unitName": "Optional. The name of the unit. This parameter is available only for output parameters of the int, float, and double data types.",
                            "size": "The number of elements in the array. Maximum value: 512. This parameter is available only for output parameters of the array data type.",
                            "step": "The step size. This parameter is unavailable for output parameters of the text or enum data type.",
                            "length": "The length of the data. Maximum value: 10240. This parameter is available only for output parameters of the text data type.",
                            "0": "The value of 0. This parameter is available only for output parameters of the bool data type.",
                            "1": "The value of 1. This parameter is available only for output parameters of the bool data type.",
                            "item": {
                                "type": "The type of the array element. This parameter is available only for output parameters of the array data type."
                            }
                        }
                    }
                }
            ],
            "method": "The name of a method that can access the service. The name is generated based on the value of the identifier parameter."
        }
    ]
}
```

You can view the extended information of the TSL model if the following settings are configured: The node type of a product is set to gateway sub-device and the gateway connection protocol is set to Modbus, OPC UA, or custom.

For example, assume that the node type of a product is set to gateway sub-device and the gateway connection protocol is set to Modbus. The extended information of the TSL model can be used in Link IoT Edge for the sub-devices. For more information, see [Link IoT Edge documentation](/intl.en-US/User Guide/Connect devices to Link IoT Edge/Official drivers/Modbus drivers.md).

Structure of the extended information of the TSL model:

**Note:** To demonstrate the full structure of the TSL model, the following example includes all parameters. However, the parameters may vary in actual scenarios. The string that follows each parameter is the description of the parameter. For the application scenarios of each parameter, see the parameter description.

```
{
  "profile": {
    productKey: "The key of the product."
  },
  "properties": [
    {
      "identifier": "The ID of the property. The ID must be unique for the product.",
      "operateType": "The operation type. Valid values: coilStatus, inputStatus, holdingRegister, and inputRegister.",
      "registerAddress": "The register address.",
      "originalDataType": {
        "type": "The type of the original data. Valid values: int16, uint16, int32, uint32, int64, uint64, float, double, string, bool, and customized (hex values in big-endian format).",
        "specs": {
          "registerCount": "The number of registers. This parameter is available only for properties of the string or customized data type.",
          "swap16": "Specifies whether to switch the high byte and low byte in the register. If so, the first and last 8 bits of the 16-bit integer in the register are swapped (byte1byte2 -> byte2byte1). This parameter is available for properties of all data types except for string and bool.",
          "reverseRegister": "Specifies whether to switch the high byte and low byte in the register. If so, the first and last 16 bits of the 32-bit integer in the register are swapped (byte1byte2byte3byte4 -> byte3byte4byte1byte2). This parameter is available for properties of all data types except for string and bool."
        }
      },
      "scaling": "The zoom factor. This parameter is available for properties of all data types except for string and bool.",
      "trigger": "The data report method. Valid values: 1 and 2, where 1 refers to reporting data at a specific time and 2 refers to reporting data changes.",
      "writeFunctionCode": "The read/write operation. The valid values of this parameter vary based on the value of the operateType parameter. If the operateType is coilStatus, the valid values are: 5 (read/write: read 0x01 and write 5x05), 15 (read/write: read 0x01 and write 0x0F), 0 (read-only 0x01), 6 (write-only 0x05), and 15 (write-only 0x0F). If the operateType parameter is set to inputStatus, the valid value is 0 (read-only 0x02). If the operateType parameter is set to holdingRegister, the valid values are: 6 (read/write: read 0x03 and write 0x06), 16 (read/write: read 0x03 and write 0x10), 0 (read-only 0x03), 6 (write-only 0x06), and 16 (write-only 0x10). If the operateType is inputRegister, the valid value is 0 (read-only 0x04).",
      "writeOnly": "Specifies whether the permission is write-only. Valid values: 0 and 1, where 0 refers to not write-only and 1 refers to write-only.",
      "pollingTime": "The collection interval. Unit: milliseconds. You do not need to specify this parameter. The collection interval of the devices is used.",
      "bitMask": "The mask. This parameter is available only for properties of the bool type. Valid values: 1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096, 8192, 16384, and 32768."
    }
  ]
}            
```

## Procedure

1.  Define features for a product in the [IoT Platform console](http://iot.console.aliyun.com/). For more information, see [Add a TSL feature](/intl.en-US/Device Management/TSL/Add a TSL feature.md) and [Batch add TSL features](/intl.en-US/Device Management/TSL/Batch add TSL features.md).
2.  Implement the TSL model of the product. For more information, see [Link SDK documentation](https://www.alibabacloud.com/help/product/93051.htm).
3.  Debug uplink and downlink connections. After the TSL model is implemented, the devices of the product can report properties and events to IoT Platform. IoT Platform can send commands to devices to set properties or call services. For more information, see [Device properties, events, and services](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Device properties, events, and services.md). You can also set the expected property values for devices in IoT Platform. For more information, [see](/intl.en-US/Device Management/Develop devices based on Alink Protocol/Desired device property values.md).
4.  View the data of each device. The data includes the reported properties, reported events, and service calls. The data is displayed in the IoT Platform console after [strong verification or weak verification](#section_peo_85g_xcn). You can log on to the [IoT Platform console](http://iot.console.aliyun.com/) and view the data on the **TSL Data** tab of the Device Details page. You can use [server-side subscription](/intl.en-US/Communications/Server-side Subscription/What is server-side subscription?.md) and [data forwarding](/intl.en-US/Communications/Data Forwarding/Overview.md) to obtain the properties, events, and the result of commands to set device properties and invoke services.

## TSL data verification

After a device reports TSL data to IoT Platform, IoT Platform verifies the data based on the specified data verification mode.

The following table describes the data verification modes.

|Mode|Applicable scenarios|Description|
|----|--------------------|-----------|
|Strong verification|If a product was created before October 14, 2020, you can use only the strong verification mode.|IoT Platform verifies all fields of the data.After verification, all data is forwarded. The data that fails to pass the verification is tagged when the data is forwarded. For more information, see [Data formats](/intl.en-US/Communications/Data formats.md).

In the [IoT Platform console](http://iot.console.aliyun.com/), the data is displayed on the **TSL Data** tab of the Device Details page. |
|Weak verification|If a product was created on or after October 14, 2020, the weak verification mode is used by default.|IoT Platform verifies only the identifier and dataType fields of the data.After verification, all data is forwarded. The data that fails to pass the verification is tagged when the data is forwarded. For more information, see [Data formats](/intl.en-US/Communications/Data formats.md).

In the [IoT Platform console](http://iot.console.aliyun.com/), the data is displayed on the **TSL Data** tab of the Device Details page. |
|Verification-free|If a product was created on or after October 14, 2020, you can switch from the weak verification mode to the verification-free mode.|IoT Platform does not verify the data. All data is forwarded.In the [IoT Platform console](http://iot.console.aliyun.com/), the data is not displayed on the **TSL Data** tab of the Device Details page. |

**Note:**

-   Verification-free mode is applicable only to users that are included in the whitelist. If you need to use the verification-free mode, [submit a ticket](https://selfservice.console.aliyun.com/ticket/createIndex) and provide your Alibaba Cloud account ID to the support team.
-   You can switch from the weak verification mode to the verification-free mode. However, you cannot switch from the verification-free mode to the weak verification mode. Proceed with caution.

To use the verification-free mode, perform the following steps:

Log on to the [IoT Platform console](http://iot.console.aliyun.com/).In the left-side navigation pane, choose **Devices** \> **Products**. On the Products page, find the product and click the product name or click View in the Actions column. On the Product Details page, click the **Define Feature** tab, and then turn off the **Data Verification** switch.

