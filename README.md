# Watch API

## 目录
iot平台端API  
[0.签名验证](#0签名验证)  
[1.批量获取设备位置](#1批量获取设备位置)  
[2.获取单设备轨迹](#2获取单设备轨迹)  
[3.批量获取报警数据](#3批量获取报警数据)  
[4.新增单设备](#4新增单设备)  
[5.批量新增设备](#5批量新增设备)  
[6.更新单设备](#6更新单设备)  
[7.批量更新设备](#7批量更新设备)  
[8.批量获取设备](#8批量获取设备)  
[9.查看单设备](#9查看单设备)   
[10.查看设备健康数据](#10查看设备健康数据)  
[11.查看设备运动数据](#11查看设备运动数据)  
[12.删除单设备](#12删除单设备)  
[13.批量删除设备](#13批量删除设备)  
[14.发送消息到单设备](#14发送消息到单设备)   
[15.群发消息到设备](#15群发消息到设备)  

商户API  
[16.webhook设置](#16webhook设置)  
[17.消息通知](#17消息通知) 

附表  
[18.消息类型](#18消息类型)   
[19.一般结构](#19一般结构)   



*注：均为http请求；非测试状态下请携带校验签名；Content-Type:application/json*

----

## iot平台端API

测试API地址：http://106.52.60.244:8196       

### 0.签名验证

调用API时需要携带以下URL参数(下方API示例为了方便没有携带，实际使用时请加上)。  
iot平台回调时亦会携带下列参数，商户可自行校验请求合法性。

|参数|类型|必选|描述
|---|---|---|---|
|secretId|string|是|商户ID，测试模式时也要携带此参数|
|signature|string|是|签名校验，测试模式时可不携带此参数|

**signature生成方法:**

md5(secret_id + secret_key + (10位unix时间戳去掉最后一位))

php:

```php
$secretId = '1234';
$secretKey = '5678';
$signature = md5($secretId . $secretKey . substr(time(), 0, -1));
// 或
$signature = md5($secretId . $secretKey . floor(time() / 10));
```


**示例:**

GET 

>/api/device/location?page=2&secretId=1234&signature=7a698gasdgw789erq

### 1.批量获取设备位置

获取的是每个设备最新的位置

方法：GET

> /api/device/location 

|参数|类型|必选|描述
|---|---|---|---|
|orderBy|string|否|排序字段，针对设备，默认为ID|
|order|string|否|排序方式，针对设备，desc或asc,默认desc|
|limitBy|string|否|限制字段，针对设备|
|start|int|否|不等式首端，针对设备，start <= limitBy|
|end|int|否|不等式尾端，针对设备，limitBy <= end|
|length|int|否|每页数据数量，针对设备，默认100|
|page|int|否|页码，针对设备，默认1|
|uid|int|否|商户自定义的UID，针对设备|
|groupId|int|否|商户自定义的分组ID，针对设备|
|deviceId|int|否|设备ID|
|duration|int|否|多少小时内的数据(从最新一条往前计时)，默认5小时|

**示例:**

>/api/device/location?page=2&orderBy=updated_at&order=asc

返回：
```json
{
    "message": "Success",
    "code": 0,
    "results": [
        {
            "deviceId": "860315001121004",
            "uid": 1234,
            "groupId": 12,
            "company": "IC",
            "phone": "13630753377",
            "center": "18642041559",
            "sos1": "18642041559",
            "sos2": null,
            "createdAt": "2020-09-08 02:41:05",
            "updatedAt": "2020-09-09 10:24:40",
            "status": 1,
            "locations": [
                {
                    "time": "2020-09-09 09:31:03",
                    "latitude": 0,
                    "longitude": 0,
                    "altitude": 100,
                    "angle": 0,
                    "speed": 0,
                    "stars": 0,
                    "gsm": 41,
                    "battery": 5,
                    "steps": 0,
                    "rolls": 0,
                    "status": "00000000",
                    "sites": [
                        "1",
                        "460",
                        "0",
                        "17237",
                        "38621",
                        "-31"
                    ],
                    "wifis": [
                        [
                            "44:f9:71:3d:bd:cc",
                            "-81"
                        ]
                    ]
                },
                {
                    "time": "2020-09-09 09:30:20",
                    "latitude": 0,
                    "longitude": 0,
                    "altitude": 100,
                    "angle": 0,
                    "speed": 0,
                    "stars": 0,
                    "gsm": 42,
                    "battery": 1,
                    "steps": 0,
                    "rolls": 0,
                    "status": "00000000",
                    "sites": [
                        "1",
                        "460",
                        "0",
                        "17237",
                        "38621",
                        "-29"
                    ],
                    "wifis": [
                        [
                            "44:f9:71:3d:bd:cc",
                            "-79"
                        ],
                        [
                            "60:ee:5c:f3:44:78",
                            "-95"
                        ]
                    ]
                }
            ]
        },
        {
            "deviceId": "860315001121053",
            "uid": null,
            "groupId": null,
            "company": "IC",
            "phone": "17247725487",
            "center": "18642041559",
            "sos1": "18642041559",
            "sos2": null,
            "createdAt": "2020-09-08 02:38:33",
            "updatedAt": "2020-09-08 02:38:33",
            "status": 1,
            "locations": [
                {
                    "time": "2020-09-09 09:06:41",
                    "latitude": 41.734551,
                    "longitude": 125.956236,
                    "altitude": 100,
                    "angle": 0,
                    "speed": 0.18,
                    "stars": 6,
                    "gsm": 34,
                    "battery": 30,
                    "steps": 409,
                    "rolls": 0,
                    "status": "00000000",
                    "sites": [
                        "1",
                        "460",
                        "0",
                        "17235",
                        "1603",
                        "-45"
                    ],
                    "wifis": []
                },
                {
                    "time": "2020-09-09 09:04:43",
                    "latitude": 41.740168,
                    "longitude": 125.966549,
                    "altitude": 100,
                    "angle": 0,
                    "speed": 0.34,
                    "stars": 8,
                    "gsm": 21,
                    "battery": 35,
                    "steps": 409,
                    "rolls": 0,
                    "status": "00000000",
                    "sites": [
                        "1",
                        "460",
                        "0",
                        "17235",
                        "8293",
                        "-71"
                    ],
                    "wifis": []
                }
            ]
        }
    ],
    "currentPage": 1,
    "lastPage": 1,
    "total": 2,
    "length": 2
}
```

[设备数据详解](#设备数据)
[位置数据详解](#位置数据)

### 2.获取单设备轨迹

方法：GET

> /api/device/location 

|参数|类型|必选|描述
|---|---|---|---|
|deviceId|string|是|设备ID|
|duration|int|否|多少时间以内的轨迹数据，默认5，单位：小时|

**示例:**

>/api/device/location?deviceId=123&duration=2

返回：
```json
{
    "result": {
        "deviceId": "860315001121004",
        "uid": 1234,
        "groupId": 12,
        "company": "IC",
        "phone": "13630753377",
        "center": "18642041559",
        "sos1": "18642041559",
        "sos2": null,
        "createdAt": "2020-09-08 02:41:05",
        "updatedAt": "2020-09-09 10:24:40",
        "status": 1,
        "locations": [
            {
                "time": "2020-09-09 13:04:55",
                "latitude": 0,
                "longitude": 0,
                "altitude": 100,
                "angle": 0,
                "speed": 0,
                "stars": 0,
                "gsm": 38,
                "battery": 15,
                "steps": 0,
                "rolls": 0,
                "status": "00000000",
                "sites": [
                    "1",
                    "460",
                    "0",
                    "17237",
                    "1093",
                    "-37"
                ],
                "wifis": [
                    [
                        "8c:a6:df:47:88:28",
                        "-90"
                    ]
                ]
            },
            {
                "time": "2020-09-09 12:55:01",
                "latitude": 0,
                "longitude": 0,
                "altitude": 100,
                "angle": 0,
                "speed": 0,
                "stars": 0,
                "gsm": 40,
                "battery": 15,
                "steps": 0,
                "rolls": 0,
                "status": "00000000",
                "sites": [
                    "1",
                    "460",
                    "0",
                    "17237",
                    "1093",
                    "-33"
                ],
                "wifis": []
            }
        ]
    },
    "message": "Success",
    "code": 0
}
```


[设备数据详解](#设备数据)
[位置数据详解](#位置数据)

### 3.批量获取报警数据

方法：GET

> /api/device/alert

|参数|类型|必选|描述
|---|---|---|---|
|orderBy|string|否|排序字段，默认为ID|
|order|string|否|排序方式，desc或asc,默认desc|
|limitBy|string|否|限制字段|
|start|int|否|不等式首端，start < limitBy < end|
|end|int|否|不等式尾端，start < limitBy < end|
|length|int|否|每页数据数量，默认100|
|page|int|否|页码，默认1|
|duration|int|否|多少时间以内的轨迹数据，默认5，单位：小时|

**示例:**

GET 

>/api/device/alert?page=2&orderBy=updated_at&order=asc

返回：
```json
{
    "message": "Success",
    "code": 0,
    "results": [
        {
            "id": 18968,
            "deviceId": "860315001121053",
            "company": "IC",
            "uid": 0,
            "groupId": null,
            "flag": "AL",
            "from": 0,
            "data": {
                "time": "2020-09-09 12:16:31",
                "latitude": 0,
                "longitude": 0,
                "altitude": 100,
                "angle": 0,
                "speed": 0,
                "stars": 0,
                "gsm": 51,
                "battery": 100,
                "steps": 834,
                "rolls": 0,
                "status": "00010000",
                "sites": [
                    "1",
                    "460",
                    "0",
                    "17237",
                    "1093",
                    "-11"
                ],
                "wifis": [
                    [
                        "44:f9:71:3d:bd:cc",
                        "-86"
                    ],
                    [
                        "8c:a6:df:47:88:28",
                        "-93"
                    ]
                ]
            },
            "createdAt": "2020-09-09 12:16:32",
            "updatedAt": "2020-09-09 12:16:32"
        },
        {
            "id": 18027,
            "deviceId": "860315001121004",
            "company": "IC",
            "uid": 1234,
            "groupId": null,
            "flag": "AL",
            "from": 0,
            "data": {
                "time": "2020-09-09 07:01:24",
                "latitude": 0,
                "longitude": 0,
                "altitude": 100,
                "angle": 0,
                "speed": 0,
                "stars": 0,
                "gsm": 50,
                "battery": 1,
                "steps": 0,
                "rolls": 0,
                "status": "00020001",
                "sites": [
                    "1",
                    "460",
                    "0",
                    "17235",
                    "32143",
                    "-13"
                ],
                "wifis": [
                    [
                        "14:75:90:a1:b6:72",
                        "-77"
                    ],
                    [
                        "06:75:90:a1:b6:72",
                        "-78"
                    ],
                    [
                        "14:75:90:a1:b7:65",
                        "-92"
                    ],
                    [
                        "14:75:90:a1:b7:2c",
                        "-95"
                    ],
                    [
                        "06:75:90:a1:b5:95",
                        "-95"
                    ]
                ]
            },
            "createdAt": "2020-09-09 07:01:26",
            "updatedAt": "2020-09-09 07:01:26"
        }
    ],
    "currentPage": 1,
    "lastPage": 33,
    "total": 66,
    "length": 2
}
```


### 4.新增单设备

方法：POST

> /api/device/device

请求参数：

|参数|类型|必选|描述
|---|---|---|---|
|deviceId|string|是|设备的IMEI或MEID，通常是设备上黏贴的条形码或二维码的值|
|company|string|是|厂商名|
|uid|int|否|商户自己平台中的用户ID|
|groupId|int|否|分组ID|
|phone|string|否|设备sim卡手机号|
|center|string|否|服务中心号码|
|sos1|string|否|求救电话1|
|sos2|string|否|求救电话2|

**示例:**

>/api/device/device

Request Body:
```json
{
  "uid": "12345",
  "deviceId": "8767865",
  "company": "IC",
  "phone": "13100000000",
  "center": "13000000000",
  "sos1": "13000000001",
  "sos2": "13000000002"
}
```

Response Body:
```json
{
    "result": "8767865",
    "message": "Success",
    "code": 0
}
```

返回的是设备ID

### 5.批量新增设备

> /api/device/devices

方法：POST

请求参数：

|参数|类型|必选|描述
|---|---|---|---|
|deviceId|string|是|设备的IMEI或MEID，通常是设备上黏贴的条形码或二维码的值|
|company|string|是|厂商名称|
|uid|int|否|商户自己平台中的用户ID|
|groupId|int|否|分组ID|
|phone|string|否|设备sim卡手机号|
|center|string|否|服务中心号码|
|sos1|string|否|求救电话1|
|sos2|string|否|求救电话2|

返回参数：

|参数|类型|描述
|---|---|---|
|success|string[]|添加成功的设备ID列表|
|failed|object|添加失败的设备ID与对应的原因|

**示例:**

POST 

>/api/device/devices

Request Body:
```json
[{
		"uid": "12346",
		"deviceId": "1238767867",
		"company": "IC",
		"phone": "13102000000",
		"center": "13000000000",
		"sos1": "13000000001",
		"sos2": "13000000002"
	}, {
		"uid": "12345",
		"deviceId": "3428767868",
		"company": "IC",
		"phone": "13100100000",
		"center": "13000000000",
		"sos1": "13000000001",
		"sos2": "13000000002"
	}
]
```

Response Body:
```json
{
    "result": {
        "success": [
            "1238767867",
            "3428767868"
        ],
        "failed": [
            {
              "deviceId": "675658",
              "message": "失败原因"
            },
            {
              "deviceId": "675658",
              "message": "失败原因"
            }
        ]
    },
    "message": "Success",
    "code": 0
}
```

### 6.更新单设备

以设备ID为基准，操作对象为设备

方法：POST

> /api/device/device/update?deviceId={deviceId}

请求参数：

|参数|类型|必选|描述
|---|---|---|---|
|uid|int|否|商户自己平台中的用户ID|
|groupId|int|否|分组ID|
|phone|string|否|设备sim卡手机号|
|center|string|否|设备可拨打的服务中心号码|
|sos1|string|否|求救电话1|
|sos2|string|否|求救电话2|

返回参数  
[设备数据详解](#设备数据)


**示例:** 

>/api/device/device/update?deviceId=1234

Request Body:
```json
{
  "uid": "12345",
  "phone": "13100000000",
  "center": "13000000000",
  "sos1": "13000000001",
  "sos2": "13000000002"
}
```

Response Body:
```json
{
    "result": {
        "deviceId": "1238767867",
        "uid": "12345",
        "groupId": null,
        "company": "IC",
        "phone": "13101200000",
        "center": "13000000000",
        "sos1": "13000000001",
        "sos2": "13000000002",
        "status": 1,
        "createdAt": "2020-09-09 13:56:40",
        "updatedAt": "2020-09-09 13:58:22"
    },
    "message": "Success",
    "code": 0
}
```

### 7.批量更新设备

方法：POST

> /api/device/devices/update

请求参数：

|参数|类型|必选|描述
|---|---|---|---|
|uid|int|否|商户自己平台中的用户ID|
|groupId|int|否|分组ID|
|phone|string|否|设备sim卡手机号|
|center|string|否|设备可拨打的服务中心号码|
|sos1|string|否|求救电话1|
|sos2|string|否|求救电话2|


**示例:**

>/api/device/devices/update

Request Body:
```json
[{
		"deviceId": "8767867",
		"uid": "12346",
		"phone": "13100000000",
		"center": "13000000000",
		"sos1": "13000000001",
		"sos2": "13000000002"
	}, {
		"deviceId": "8767868",
		"uid": "12345",
		"phone": "13100000000",
		"center": "13000000000",
		"sos1": "13000000001",
		"sos2": "13000000002"
	}
]
```

Response Body:
```json
{
    "result": {
        "success": [
            "8767868"
        ],
        "failed": [
            {
              "deviceId": "675658",
              "message": "失败原因"
            },
            {
              "deviceId": "675658",
              "message": "失败原因"
            }
        ]
    },
    "message": "Success",
    "code": 0
}
```


### 8.批量获取设备

> /api/device/device

方法：GET

|参数|类型|必选|描述
|---|---|---|---|
|orderBy|string|否|排序字段，默认为ID|
|order|string|否|排序方式，desc或asc,默认desc|
|limitBy|string|否|限制字段|
|start|int|否|不等式首端，start < limitBy < end|
|end|int|否|不等式尾端，start < limitBy < end|
|length|int|否|每页数据数量，默认100|
|uid|int|否|商户自定义的UID|
|groupId|int|否|商户自定义的分组ID|
|deviceId|int|否|设备ID|
|page|int|否|页码，默认1|

**示例:**

GET 

>/api/device/device?page=2&orderBy=updated_at&order=asc

返回：
```json
{
    "message": "Success",
    "code": 0,
    "results": [
        {
            "deviceId": "3428767868",
            "uid": 12345,
            "groupId": null,
            "company": "IC",
            "phone": "13100100000",
            "center": "13000000000",
            "sos1": "13000000001",
            "sos2": "13000000002",
            "status": 1,
            "createdAt": "2020-09-09 13:56:40",
            "updatedAt": "2020-09-09 13:56:40"
        },
        {
            "deviceId": "1238767867",
            "uid": 12345,
            "groupId": null,
            "company": "IC",
            "phone": "13101200000",
            "center": "13000000000",
            "sos1": "13000000001",
            "sos2": "13000000002",
            "status": 1,
            "createdAt": "2020-09-09 13:56:40",
            "updatedAt": "2020-09-09 13:58:22"
        }
    ],
    "currentPage": 1,
    "lastPage": 3,
    "total": 5,
    "length": 2
}
```

[设备数据详解](#设备数据)

### 9.查看单设备

> /api/device/device/{deviceId}

方法：GET

**示例:**

GET 

>/api/device/device/89a7dsfg987as6d9a8

返回：
```json
{
    "result": {
        "deviceId": "3428767868",
        "uid": 12345,
        "groupId": null,
        "company": "IC",
        "phone": "13100100000",
        "center": "13000000000",
        "sos1": "13000000001",
        "sos2": "13000000002",
        "status": 1,
        "createdAt": "2020-09-09 13:56:40",
        "updatedAt": "2020-09-09 13:56:40"
    },
    "message": "Success",
    "code": 0
}
```

[设备数据详解](#设备数据)


### 10.查看设备健康数据

> /api/device/health/{deviceId} 

方法：GET

|参数|类型|必选|描述
|---|---|---|---|
|length|int|否|每页数据数量，默认100|
|page|int|否|页码，默认1|
|duration|int|否|多少时间以内的健康数据，默认5，单位：小时|

**示例:**

GET 

>/api/device/health/a8sd7g6a98dsf?duration=100

返回：
```json
{
    "message": "Success",
    "code": 0,
    "results": [
        {
            "heart": 75,
            "createdAt": "2020-09-09 13:18:12"
        },
        {
            "blood": [
                143,
                81
            ],
            "createdAt": "2020-09-09 13:18:12"
        }
    ],
    "currentPage": 1,
    "lastPage": 9,
    "total": 18,
    "length": 2
}
```

[健康数据详解](#健康数据)


### 11.查看设备运动数据

> /api/device/sport/{deviceId}

方法：POST

|参数|类型|必选|描述
|---|---|---|---|
|length|int|否|每页数据数量，默认100|
|page|int|否|页码，针对设备，默认1|
|duration|int|否|多少时间以内的健康数据，默认5，单位：小时|


**示例:**

GET 

>/api/device/health/a8sd7g6a98dsf?duration=100

返回：
```json
{
    "message": "Success",
    "code": 0,
    "results": [
        {
            "date": "2020-09-09",
            "steps": 834,
            "rolls": 0,
            "battery": 95
        },
        {
            "date": "2020-09-09",
            "steps": 834,
            "rolls": 0,
            "battery": 95
        }
    ],
    "currentPage": 1,
    "lastPage": 30,
    "total": 60,
    "length": 2
}
```

[运动数据详解](#运动数据)


### 12.删除单设备

方法：POST 

> /api/device/device/delete

|参数|类型|必选|描述|
|---|---|---|---|
|deviceId|string|是|设备ID|

**示例:**

>/api/device/device/delete?deviceId=1234

Response Body:
```json
{
    "result": "8767865",
    "message": "Success",
    "code": 0
}
```


### 13.批量删除设备

方法：POST

> /api/device/devices/delete

参数  
|参数|类型|必选|描述|
|---|---|---|---|
|deviceIds|string[]|是|设备ID的列表|


返回参数：

|参数|类型|描述
|---|---|---|
|success|string[]|删除成功的设备ID列表|
|failed|string[]|删除失败的设备ID列表|

**示例:**

POST

>/api/device/devices/delete

Request Body:
```json
{
    "deviceIds":  ["8767867", "8767868"]
}
```

Response Body:
```json
{
    "results": {
        "success": [
            "8767868"
        ],
        "failed": [
            {
              "deviceId": "675658",
              "message": "失败原因"
            },
            {
              "deviceId": "675658",
              "message": "失败原因"
            }
        ]
    },
    "message": "Success",
    "code": 0
}
```


### 14.发送消息到单设备

方法：POST

> /api/device/event

请求参数：

|参数|类型|必选|描述
|---|---|---|---|
|deviceId|string|是|设备的IMEI或MEID，通常是设备上黏贴的条形码或二维码的值|
|flag|string|是|消息类型，详情请看：[消息类型](#18消息类型)|
|data|string|否|消息内容，可能是base64字符串，通过下方encoded字段判断，内容是否需要编码，请查看：[消息类型](#18消息类型)|
|encoded|bool|否|data字段是否是base64字符串，false：不是, true：是，默认：false|

**示例:**

POST 

>/api/device/event

Request Body:
```json
{
    "deviceId":"860315001121053",
    "flag":"FIND",
    "data":null,
    "encoded": false
}
```

Response Body:
```json
{
    "result": 765371,
    "message": "Success",
    "code": 0
}
```


### 15.群发消息到设备

> /api/device/events

方法：POST

请求参数：

|参数|类型|必选|描述
|---|---|---|---|
|deviceIds|string[]|是|ID列表，设备的IMEI或MEID，通常是设备上黏贴的条形码或二维码的值|
|flag|string|是|消息类型，详情请看：[消息类型](#18消息类型)|
|groupId|int|否|分组|
|data|string|否|消息内容，可能是base64字符串，通过下方encoded字段判断，内容是否需要编码，请查看：[消息类型](#18消息类型)|
|encoded|bool|否|data字段是否是base64字符串，false：不是, true：是，默认：false|


返回参数：

|参数|类型|描述
|---|---|---|
|success|string[]|发送成功的设备ID列表|

**示例:**

POST 

>/api/device/events

Request Body:
```json
{
    "deviceIds": [
      "860315001121053",
      "860315001121054"
    ],
    "all": false,
    "data": null,
    "flag": "FIND",
    "encoded": false
}
```

Response Body:
```json
{
    "result": {
        "success": [
            "860315001121054"
        ]
    },
    "message": "Success",
    "code": 0
}
```

### 16.webhook设置

iot平台通过商户的webhook端点向商户服务端推送消息，端点可通知管理员修改。

推送消息采用POST方法，内容格式：application/json


### 17.消息通知

iot平台接收到设备消息时，同步通知商户

方法 POST 

>**endpoint**/notify

示例：

iot平台端：

POST
>**endpoint**/notify

Request Body:
```json
{
	"id": 14197,
	"device": {
		"deviceId": "860315001121053",
		"uid": null,
		"groupId": null,
		"company": "IC",
		"phone": "17247725487",
		"center": "18642041559",
		"sos1": "18642041559",
		"sos2": null,
		"status": 1,
		"createdAt": "2020-09-08 02:38:33",
		"updatedAt": "2020-09-08 02:38:33"
	},
	"flag": "AL",
	"from": 0,
	"data": {
		"time": "2020-09-08 05:26:57",
		"latitude": 0,
		"longitude": 0,
		"altitude": 100,
		"angle": 0,
		"speed": 0,
		"stars": 0,
		"gsm": 46,
		"battery": 20,
		"steps": 0,
		"rolls": 0,
		"status": "00000000",
		"sites": ["1", "460", "0", "17237", "1093", "-21"],
		"wifis": [
			["44:f9:71:3d:bd:cc", "-80"],
			["8c:a6:df:47:88:28", "-94"]
		]
	},
	"createdAt": "2020-09-08 13:26:59",
	"updatedAt": "2020-09-08 13:26:59"
}
```

[设备数据详解](#设备数据`)
[位置数据详解](#位置数据`)
[消息类型详解](#18消息类型`)

商户端：

Response Body:
>Success

商户端回复字符串 *Success* 代表接收成功。

### 18.消息类型

**设备发出:**  

|类型|描述|
|---|---|
|LK|心跳包|
|PING|绑定检查|
|KA|运动数据（步数、翻滚次数、电量）|
|UD|位置，设备按一定时间间隔自动上报的位置|
|UD2|补传的自动位置|
|CRUD|位置，响应CR命令发出的消息|
|AL|SOS上报|
|Time|获取时间|
|WT|获取天气|
|img|上传拍照|
|VOICE|上传音频|
|WG|请求位置|
|temp|上传体温|
|blood|上传血压|
|oxygen|上传血氧|

**平台发出：**  

|类型|值|描述
|---|---|---|
|UPLOAD|秒数|设置上传间隔|
|CENTER|电话号|服务中心号码设置|
|MONITOR|无|监听，静默拨打服务中心号码|
|SOS|电话号1,电话号2|设置紧急联系人|
|FACTORY|无|恢复出厂设置|
|SOSSMS|0或1|sos短信开关|
|LOWBAT|0或1|低电压报警开关|
|VERNO|无|版本查询|
|CR|无|立即定位指令，返回的是CRUD|
|TK|AMR数据|需要把AMR音频数据用base64编码|
|POWEROFF|无|关机指令|
|REMOVE|0或1|取下手环报警开关|
|PEDO|0或1|计步功能开关|
|SILENCETIME|7:30-21:10,7:30-21:10,7:30-21:10|免打扰设置|
|SILENCETIME2|-|免打扰设置2|
|SLEEPTIME|23:30-7:30|翻身检测时间设置，此时间段内开启翻身检测|
|FIND|无|响铃|
|REMIND|-|闹钟设置|
|TK|AMR数据，需要base64编码|语音消息|


### 19.一般结构

#### 设备数据

|参数|类型|描述
|---|---|---|
|deviceId|string|设备的IMEI或MEID，通常是设备上黏贴的条形码或二维码的值|
|company|string|厂商名|
|uid|int|商户自己平台中的用户ID|
|groupId|int|分组ID|
|phone|string|设备sim卡手机号|
|center|string|服务中心号码|
|sos1|string|求救电话1|
|sos2|string|求救电话2|


```json
{
    "deviceId": "860315001121004",
    "uid": 12346,
    "groupId": null,
    "company": "IC",
    "phone": "13630753377",
    "center": "10086",
    "sos1": "10086",
    "sos2": "18642041559",
    "createdAt": "2020-09-05 17:25:55",
    "updatedAt": "2020-09-05 17:31:12"
}

```

#### 位置数据

|字段|类型|可空|描述
|---|---|---|---|
|time|datetime|否|创建时间(世界时)，格式：2006-01-02 15:04:05|
|latitude|float|是|纬度|
|longitude|float|是|经度|
|angle|float|是|朝向角度|
|speed|float|是|速度，m/s|
|stars|int|是|卫星数量|
|gsm|int|是|gsm信号强度|
|battery|int|否|电池电量 0-100|
|steps|int|是|今日步数|
|rolls|int|是|翻滚次数|
|status|string|否|状态|
|sites|array|是|基站数据|
|wifis|array|是|附近wifi指纹数据，可用第三方lbs服务定位|

```json
{
    "time": "2020-09-07 05:46:40",
    "latitude": 0,
    "longitude": 0,
    "altitude": 100,
    "angle": 0,
    "speed": 0,
    "stars": 0,
    "gsm": 26,
    "battery": 30,
    "steps": 440,
    "rolls": 0,
    "status": "00000000",
    "sites": [
        "1",
        "460",
        "0",
        "17237",
        "1093",
        "-61"
    ],
    "wifis": [
        [
            "44:f9:71:3d:bd:cc",
            "-88"
        ]
    ]
}
```

#### 健康数据

- **blood** 血压

```json
[
  120,
  75
]
```

第一个元素是收缩压，第二个元素是舒张压

- **heart** 每分钟心率

心率是一个整型数字


- **oxygen** 血氧含量

血氧含量是一个整型数字

#### 运动数据

|参数|类型|描述
|---|---|---|
|date|date|日期|
|steps|int|步数|
|rolls|int|翻滚次数|
|battery|int|电量 0-100|

```json
{
    "date": "2020-09-07",
    "steps": 38,
    "rolls": 123,
    "battery": 47
}
```