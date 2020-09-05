# Watch API

## 目录

iot平台端API  
- [x] 0.签名验证  
- [x] 1.批量获取设备位置 (需要优化数据格式，需要完善筛选条件，位置信息获取失败) 
- [ ] 2.获取单个设备轨迹  
- [x] 3.获取报警数据  (需要优化数据格式，需要完善筛选条件，位置信息获取失败)
- [x] 4.新增设备  
- [x] 5.批量新增设备  
- [x] 6.修改设备  
- [x] 7.批量修改设备  
- [x] 8.批量获取设备 
- [x] 9.查看单个设备详情 
- [x] 10.查看设备健康数据
- [x] 11.删除设备  
- [x] 12.批量删除设备  
- [ ] 13.发送消息到单个设备  
- [ ] 14.群发消息到设备  

商户API  
- [x] 15.webhook端点设置  
- [ ] 16.SOS报警通知 



*注：均为http请求；非测试状态下请携带校验签名；Content-Type:application/json*

----

## iot平台端API

### 0.签名验证

调用API时需要携带以下URL参数(下方API示例为了方便没有携带，实际使用时请加上)。  
iot平台回调时亦会携带下列参数，商户可自行校验合请求法性。

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
|orderBy|string|否|排序字段，默认为ID|
|order|string|否|排序方式，desc或asc,默认desc|
|limitBy|string|否|限制字段|
|start|int|否|不等式首端，start < limitBy < end|
|end|int|否|不等式尾端，start < limitBy < end|
|length|int|否|每页数据数量，默认100|
|page|int|否|页码，默认1|

**示例:**

GET 

>/api/device/locations?page=2&orderBy=updated_at&order=asc

返回：
```json
{
	"code": 0,
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


### 4.新增设备

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

### 6.修改设备

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


### 8.删除设备


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


### 9.批量删除设备

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




### 12.webhook端点设置

iot平台通过商户的webhook端点向商户服务端推送消息，端点可在商户后台修改。

端点格式示例：

> https://abc.def/api/watch/notify

商户需要实现此API：

Get
> https://abc.def/api/watch/ping

返回字符串
>pong


### 13.SOS报警通知

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

