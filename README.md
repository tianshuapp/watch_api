# Watch API

## 目录
iot平台端API

1.批量获取单点位置

2.获取单个设备轨迹

3.获取报警数据


## iot平台端API

### 1.批量获取单点位置

> /api/locations 

方法：GET

|参数|类型|必选|描述
|---|---|---|---|
|view|int|否|查看次数，默认获取查看次数为0的数据|
|orderBy|string|否|排序字段，默认为ID|
|order|string|否|排序方式，desc或asc,默认desc|
|length|int|否|每页数据数量，默认100|
|page|int|否|页码，默认1|

**示例:**

GET 

>/api/locations?page=2&orderBy=updated_at&order=asc

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
