# QueryDeviceFile {#reference_yfg_jtf_2hb .reference}

You can call this operation to query the information about a file that is uploaded to IoT Platform from the specified device.

## Request parameters {#section_oxs_xtf_2hb .section}

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|Action|String |Yes|The operation that you want to perform. Set the value to QueryDeviceFile.|
|IotId|String|No|The unique identifier of the device.**Note:** If you provide this parameter, you do not need to provide the ProductKey or DeviceName parameters. As the GUID of the device, IotId corresponds to the combination of ProductKey and DeviceName. If you provide both IotId and the combination of ProductKey and DeviceName, IotId takes precedence. This parameter cannot be left empty.

|
|ProductKey|String|No|The key of the product to which the device belongs. **Note:** If you provide this parameter, you must also provide the DeviceName parameter.

|
|DeviceName|String|No|The device name. **Note:** If you provide this parameter, you must also provide the ProductKey parameter.

|
|FileId|String |Yes|The file identifier.|
|Common request parameters|-|Yes|See [Common parameters](reseller.en-US/Developer Guide (Cloud)/API reference/Common parameters.md#).|

## Response parameters {#section_m2r_35f_2hb .section}

|Parameter|Type|Description|
|:--------|:---|:----------|
|RequestId|String|The GUID generated by Alibaba Cloud for the request.|
|Success|Boolean|Indicates whether the call is successful. A value of true indicates that the call is successful. A value of false indicates that the call has failed.|
|ErrorMessage|String|The error message returned when the call fails.|
|Code|String|The error code returned when the call fails. For more information about error codes, see [Error codes](reseller.en-US/Developer Guide (Cloud)/API reference/Error codes.md#).|
|Data|Data|The file information returned when the call is successful. For more information, see the following table: [Parameters in Data](#).|

|Parameter|Type|Description|
|:--------|:---|:----------|
|FileId|String|The file identifier.|
|Name|String|The file name.|
|Size|String|The file size.|
|UtcCreatedOn|String|The time when the file was created.|
|DownloadUrl|String|The URL from which the file can be downloaded.|

## Examples {#section_drn_l5f_2hb .section}

**Sample request**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceFile
&ProductKey=al********
&DeviceName=deviceName1
&FileId=6UCo1SqbqnQEoh9aKqD******
&Common request parameters
```

**Sample responses**

-   JSON format

    ```
    {
      "RequestId": "93C5276D-5C8A-40D9-BFD6-4BD5B8C1A08F",
      "Data": {
        "Name": "testFile3.txt",
        "DownloadUrl": "http://iotx-file-store.oss-cn-shanghai.aliyuncs.com/device_file/A849E4E5CFF64804A18D9384AC9D****/aGEKIpp5NAGxdP2oo90000****/testFile3.txt?Expires=1553162075&OSSAccessKeyId=LTAIYLScbHiV****&Signature=%2F88xdEFPukJ****%2F8****%2Bdv3io%3D",
        "FileId": "6UCo1SqbqnQEoh9aKqDQ01****",
        "UtcCreatedOn": "2019-03-21T08:45:42.000Z",
        "Size": "102400"
      },
      "Success": true
    }
    ```

-   XML format

    ```
    <? xml version="1.0" encoding="UTF-8" ? >
    	<RequestId>93C5276D-5C8A-40D9-BFD6-4BD5B8C1A08F</RequestId>
    	<Data>
    		<Name>testFile3.txt</Name>
    		<DownloadUrl>http://iotx-file-store.oss-cn-shanghai.aliyuncs.com/device_file/A849E4E5CFF64804A18D9384AC9D****/aGEKIpp5NAGxdP2oo90000****/testFile3.txt?Expires=1553162075&amp;OSSAccessKeyId=LTAIYLScbHiV****&amp;Signature=%2F88xdEFPukJ****%2F8****%2Bdv3io%3D</DownloadUrl>
    		<FileId>6UCo1SqbqnQEoh9aKqDQ01****</FileId>
    		<UtcCreatedOn>2019-03-21T08:45:42.000Z</UtcCreatedOn>
    		<Size>102400</Size>
    	</Data>
    	<Success>true</Success>
    ```


