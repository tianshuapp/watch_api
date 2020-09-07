# Watch API

## 目录

iot平台端API  
- [x] [0.签名验证](#0签名验证)  
- [ ] [1.批量获取设备位置](#1批量获取设备位置)
- [ ] [2.获取单个设备轨迹](#2.获取单个设备轨迹)  
- [ ] [3.获取报警数据](#3获取报警数据)  
- [ ] [4.新增单个设备](#4新增单个设备)  
- [ ] [5.批量新增设备](#5批量新增设备)  
- [ ] [6.更新单个设备](#6更新单个设备)  
- [ ] [7.批量更新设备](#7批量更新设备)  
- [ ] [8.批量获取设备](#8批量获取设备) 
- [ ] [9.查看单个设备](#9查看单个设备) 
- [ ] [10.查看单个设备健康数据](#10查看单个设备健康数据)
- [ ] [11.批量查看设备健康数据](#11批量查看设备健康数据)
- [ ] [12.删除设备](#12删除设备)  
- [ ] [13.批量删除设备](#13批量删除设备)  
- [ ] [14.发送消息到单个设备](#14发送消息到单个设备) 
- [ ] [15.群发消息到设备](#15群发消息到设备)  

商户API  
- [ ] 17.webhook端点设置  
- [ ] 18.SOS报警通知 



*注：均为http请求；非测试状态下请携带校验签名；Content-Type:application/json*

----

## iot平台端API

服务器地址：http://106.52.60.244:8195       

### 0.签名验证

调用API时需要携带以下URL参数(下方API示例为了方便没有携带，实际使用时请加上)。  
iot平台回调时亦会携带下列参数，商户可自行校验请求合法性。

|参数|类型|必选|描述
|---|---|---|---|
|secretId|string|是|商户ID，测试模式时也要携带此参数|
|signature|string|是|签名校验，测试模式时可不携带此参数|

**signature生成方法:**

md5(secret_id + secret_key + (unix时间戳去掉最后一位))

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

> /api/device/location 

方法：GET

|参数|类型|必选|描述
|---|---|---|---|
|orderBy|string|否|排序字段，针对设备，默认为ID|
|order|string|否|排序方式，针对设备，desc或asc,默认desc|
|limitBy|string|否|限制字段，针对设备|
|start|int|否|不等式首端，针对设备，start <= limitBy|
|end|int|否|不等式尾端，针对设备，limitBy <= end|
|length|int|否|每页数据数量，针对设备，默认100|
|page|int|否|页码，针对设备，默认1|
|locationCount|int|否|每个设备获取多少个位置数据，默认5个|

**示例:**

GET 

>/api/device/locations?page=2&orderBy=updated_at&order=asc

返回：
```json
{
    "data": {
        "list": [
            {
                "deviceId": "860315001121004",
                "uid": 12346,
                "groupId": null,
                "phone": "13630753377",
                "center": "10086",
                "sos1": "10086",
                "sos2": "18642041559",
                "createdAt": "2020-09-05 17:25:55",
                "updatedAt": "2020-09-05 17:31:12",
                "locations": []
            },
            {
                "deviceId": "860315001121053",
                "uid": 12345,
                "groupId": null,
                "phone": "17247725487",
                "center": "10086",
                "sos1": "18642041559",
                "sos2": "10086",
                "createdAt": "2020-09-05 15:42:37",
                "updatedAt": "2020-09-05 15:42:37",
                "locations": [
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
                    },
                    {
                        "time": "2020-09-07 05:39:40",
                        "latitude": 41.705539,
                        "longitude": 125.914325,
                        "altitude": 100,
                        "angle": 58.1,
                        "speed": 1.62,
                        "stars": 7,
                        "gsm": 55,
                        "battery": 30,
                        "steps": 338,
                        "rolls": 0,
                        "status": "00000000",
                        "sites": [
                            "1",
                            "460",
                            "0",
                            "17237",
                            "1093",
                            "-3"
                        ],
                        "wifis": [
                            [
                                "00:27:1d:fd:02:7c",
                                "-95"
                            ]
                        ]
                    },
                    {
                        "time": "2020-09-07 05:39:11",
                        "latitude": 41.704326,
                        "longitude": 125.908823,
                        "altitude": 100,
                        "angle": 117.4,
                        "speed": 0.98,
                        "stars": 10,
                        "gsm": 58,
                        "battery": 30,
                        "steps": 338,
                        "rolls": 0,
                        "status": "00000000",
                        "sites": [
                            "1",
                            "460",
                            "0",
                            "17237",
                            "1093",
                            "3"
                        ],
                        "wifis": [
                            [
                                "d8:d8:66:aa:45:cc",
                                "-96"
                            ]
                        ]
                    },
                    {
                        "time": "2020-09-07 05:32:04",
                        "latitude": 0,
                        "longitude": 0,
                        "altitude": 100,
                        "angle": 0,
                        "speed": 0,
                        "stars": 0,
                        "gsm": 33,
                        "battery": 30,
                        "steps": 157,
                        "rolls": 0,
                        "status": "00000000",
                        "sites": [
                            "1",
                            "460",
                            "0",
                            "17237",
                            "1093",
                            "-47"
                        ],
                        "wifis": []
                    },
                    {
                        "time": "2020-09-07 05:27:18",
                        "latitude": 0,
                        "longitude": 0,
                        "altitude": 100,
                        "angle": 0,
                        "speed": 0,
                        "stars": 0,
                        "gsm": 40,
                        "battery": 30,
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
                        "wifis": [
                            [
                                "44:f9:71:3d:bd:cc",
                                "-88"
                            ]
                        ]
                    }
                ]
            }
        ],
        "meta": {
            "currentPage": 1,
            "lastPage": 1,
            "total": 2,
            "length": 10
        }
    },
    "message": "Success",
    "code": 0
}
```

位置数据结构

|参数|类型|可空|描述
|---|---|---|---|
|time|string|否|创建时间|
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

### 2.获取单设备轨迹

> /api/trail/[deviceid] 

方法：GET

|参数|类型|必选|描述
|---|---|---|---|
|duration|int|否|多少时间以内的轨迹数据，默认2，单位：小时|

**示例:**

GET 

>/api/device/trail/123?duration=2

返回：
```json
{
	"code": 1,
	"message": "Success",
	"data": [{
		"deviceId": 123,
		"userId": 235123,
		"location": 123
	}, {
		"deviceId": 125,
		"userId": 235156,
		"location": 2346
	}]
}
```

### 3.批量获取报警数据

> /api/device/alert

方法：GET

|参数|类型|必选|描述
|---|---|---|---|
|orderBy|string|否|排序字段，默认为ID|
|order|string|否|排序方式，desc或asc,默认desc|
|limitBy|string|否|限制字段|
|start|int|否|不等式首端，start < limitBy < end|
|end|int|否|不等式尾端，start < limitBy < end|
|length|int|否|每页数据数量，默认100|
|page|int|否|页码，默认1|

**示例:**

GET 

>/api/device/alert?page=2&orderBy=updated_at&order=asc

返回：
```json
{
	"code": 1,
	"message": "Success",
	"data": [{
		"deviceId": 123,
		"userId": 235123,
		"location": 123
	}, {
		"deviceId": 125,
		"userId": 235156,
		"location": 2346
	}]
}
```


### 4.新增单个设备

> /api/device/device

方法：POST

请求参数：

|参数|类型|必选|描述
|---|---|---|---|
|deviceId|string|是|设备的IMEI或MEID，通常是设备上黏贴的条形码或二维码的值|
|uid|int|否|商户自己平台中的用户ID|
|groupId|int|否|分组ID|
|phone|string|否|设备sim卡手机号|
|center|string|否|设备可拨打的服务中心号码|
|sos1|string|否|求救电话1|
|sos2|string|否|求救电话2|

返回参数：

|参数|类型|描述
|---|---|---|
|data|string|添加成功的设备ID|

**示例:**

POST 

>/api/device/device

Request Body:
```json
{
  "uid": "12345",
  "deviceId": "8767865",
  "phone": "13100000000",
  "center": "13000000000",
  "sos1": "13000000001",
  "sos2": "13000000002"
}
```

Response Body:
```json
{
    "data": "87678asdf65",
    "message": "Success",
    "code": 0
}
```

### 5.批量新增设备

> /api/device/device

方法：POST

请求参数：

|参数|类型|必选|描述
|---|---|---|---|
|deviceId|string|是|设备的IMEI或MEID，通常是设备上黏贴的条形码或二维码的值|
|uid|int|否|商户自己平台中的用户ID|
|groupId|int|否|分组ID|
|phone|string|否|设备sim卡手机号|
|center|string|否|设备可拨打的服务中心号码|
|sos1|string|否|求救电话1|
|sos2|string|否|求救电话2|

返回参数：

|参数|类型|描述
|---|---|---|
|data|string|添加成功的设备ID|

**示例:**

POST 

>/api/device/devices

Request Body:
```json
[{
		"uid": "12346",
		"deviceId": "8767867",
		"phone": "13100000000",
		"center": "13000000000",
		"sos1": "13000000001",
		"sos2": "13000000002"
	}, {
		"uid": "12345",
		"deviceId": "8767868",
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
    "data": {
        "success": [
            "8767868"
        ],
        "failed": {
            "8767867": "设备已经添加过，无法再次添加"
        }
    },
    "message": "Success",
    "code": 0
}
```

### 6.更新单个设备

以设备ID为基准，操作对象为设备

> /api/device/device/{deviceId}

方法：PUT

请求参数：

|参数|类型|必选|描述
|---|---|---|---|
|uid|int|否|商户自己平台中的用户ID|
|groupId|int|否|分组ID|
|phone|string|否|设备sim卡手机号|
|center|string|否|设备可拨打的服务中心号码|
|sos1|string|否|求救电话1|
|sos2|string|否|求救电话2|

返回参数：

|参数|类型|描述
|---|---|---|
|data|string|修改成功的设备ID|

**示例:**

POST 

>/api/device/device/1234

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
    "data": "1234",
    "message": "Success",
    "code": 0
}
```

### 7.批量更新设备

> /api/device/devices

方法：PUT

请求参数：

|参数|类型|必选|描述
|---|---|---|---|
|deviceId|string|是|设备的IMEI或MEID，通常是设备上黏贴的条形码或二维码的值|
|uid|int|否|商户自己平台中的用户ID|
|groupId|int|否|分组ID|
|phone|string|否|设备sim卡手机号|
|center|string|否|设备可拨打的服务中心号码|
|sos1|string|否|求救电话1|
|sos2|string|否|求救电话2|

返回参数：

|参数|类型|描述
|---|---|---|
|data|string|添加成功的设备ID|

**示例:**

PUT

>/api/device/devices

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
    "data": {
        "success": [
            "8767868"
        ],
        "failed": {
            "8767867": "设备ID已经存在，无法修改"
        }
    },
    "message": "Success",
    "code": 0
}
```

### 8.查看设备列表

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
|page|int|否|页码，默认1|

**示例:**

GET 

>/api/device/device?page=2&orderBy=updated_at&order=asc

返回：
```json
{
	"code": 0,
	"message": "Success",
	"data": {
		"meta": {
			"currentPage": 1,
			"total": 123,
			"length": 10,
			"lastPge": 4
		},
		"list": [{
			"deviceId": "89a7dsfg987as6d9a8",
			"uid": 235123,
			"phone": 235123,
			"center": 235123,
			"sos1": 235123,
			"sos2": 235123
		}, {
			"deviceId": "89a7dsfg987as6d9a8",
			"uid": 235123,
			"phone": 235123,
			"center": 235123,
			"sos1": 235123,
			"sos2": 235123
		}]
	}
}
```


### 9.查看设备详情

> /api/device/device/{deviceId}

方法：GET

**示例:**

GET 

>/api/device/device/89a7dsfg987as6d9a8?page=2&orderBy=updated_at&order=asc

返回：
```json
{
	"code": 0,
	"message": "Success",
	"data": {
        "deviceId": "89a7dsfg987as6d9a8",
        "uid": 235123,
        "phone": 235123,
        "center": 235123,
        "sos1": 235123,
        "sos2": 235123
    }
}
```



### 10.查看单个设备健康数据

> /api/device/health/{deviceId} 

方法：GET

|参数|类型|必选|描述
|---|---|---|---|
|duration|int|否|多少时间以内的健康数据，默认2，单位：小时|

**示例:**

GET 

>/api/device/health/a8sd7g6a98dsf?duration=100

返回：
```json
{
	"code": 0,
	"message": "Success",
	"data": [{
      "device": {
		"deviceId": 123,
		"userId": 235123,
		"location": 123
      },
      "healths": [
        {
          "blood": 90,
          "createdAt": "2006-01-02 15:04:05"
        },
        {
          "heart": 90,
          "createdAt": "2006-01-02 15:04:05"
        },
        {
          "blood": 90,
          "createdAt": "2006-01-02 15:01:05"
        },
        {
          "oxygen": 90,
          "createdAt": "2006-01-02 15:04:05"
        }
      ]
	}]
}
```



### 11.批量查看设备健康数据

> /api/device/healths 

方法：POST

|参数|类型|必选|描述
|---|---|---|---|
|duration|int|否|多少时间以内的健康数据，默认2，单位：小时|
|deviceIds|string[]|是|设备ID数组|


**示例:**

GET 

>/api/device/health/a8sd7g6a98dsf?duration=100

返回：
```json
{
	"code": 0,
	"message": "Success",
	"data": [{
      "device": {
		"deviceId": 123,
		"userId": 235123
      },
      "healths": [
        {
          "blood": 90,
          "createdAt": "2006-01-02 15:04:05"
        },
        {
          "heart": 90,
          "createdAt": "2006-01-02 15:04:05"
        },
        {
          "blood": 90,
          "createdAt": "2006-01-02 15:01:05"
        },
        {
          "oxygen": 90,
          "createdAt": "2006-01-02 15:04:05"
        }
      ]
	},{
      "device": {
        "deviceId": 124,
        "userId": 235124
      },
      "healths": [
          {
            "blood": 90,
            "createdAt": "2006-01-02 15:04:05"
          },
          {
            "heart": 90,
            "createdAt": "2006-01-02 15:04:05"
          },
          {
            "blood": 90,
            "createdAt": "2006-01-02 15:01:05"
          },
          {
            "oxygen": 90,
            "createdAt": "2006-01-02 15:04:05"
          }
      ]
    }]
}
```


### 12.删除单个设备


> /api/device/device/{deviceId}

方法：DELETE

**示例:**

DELETE 

>/api/device/device/1234

Response Body:
```json
{
    "data": "1234",
    "message": "Success",
    "code": 0
}
```


### 13.批量删除设备

> /api/device/devices/delete

方法：DELETE

返回参数：

|参数|类型|描述
|---|---|---|
|data|Array|删除成功的设备ID列表|

**示例:**

PUT

>/api/device/devices?deviceIds=8767867,8767868

Request Body:
```json
[
  {
    "deviceId": "8767867" 
  },
  {
    "deviceId": "8767868" 
  }
]
```

Response Body:
```json
{
    "data": {
        "success": [
            "8767868"
        ],
        "failed": {
            "8767867": "有SOS消息，不可删除"
        }
    },
    "message": "Success",
    "code": 0
}
```


### 14.发送消息到单个设备

> /api/device/event

方法：POST

请求参数：

|参数|类型|必选|描述
|---|---|---|---|
|deviceId|string|是|设备的IMEI或MEID，通常是设备上黏贴的条形码或二维码的值|
|flag|string|是|消息类型，详情请看：|
|uid|int|否|商户自己平台中的用户ID|
|groupId|int|否|分组ID|
|data|string|否|消息内容，内容字节数组转换的base64字符串|


返回参数：

|参数|类型|描述
|---|---|---|
|data|int|添加成功消息ID|

**示例:**

POST 

>/api/device/event

Request Body:
```json
{
    "deviceId":"860315001121053",
    "uid":123,
    "groupId":123,
    "data":null,
    "flag":"FIND"
}
```

Response Body:
```json
{
    "data": 765371,
    "message": "Success",
    "code": 0
}
```


### 15.群发消息到设备

### 16.webhook端点设置

iot平台通过商户的webhook端点向商户服务端推送消息，端点可在商户后台修改。

端点格式示例：

> https://abc.def/api/watch/notify

商户需要实现此API：

Get
> https://abc.def/api/watch/ping

返回字符串
>pong


### 17.SOS报警通知

iot接收到设备SOS消息时，同步通知商户

POST 

>**endpoint**/alert

示例：

iot平台端：

POST
>**endpoint**/alert

Request Body:
```json
{
  "uid": "12345",
  "deviceId": "8767865",
  "location": {
    "longitude": 123.4123,
    "latitude": 65.1234,
    "altitude": 351.1,
    "angle": 195.1,
    "speed": 0.0
  },
  "health": {
    "heart": 100,
    "blood": "100/140",
    "oxygen": 100
  }
}
```

商户端：

Response Body:
>Success

