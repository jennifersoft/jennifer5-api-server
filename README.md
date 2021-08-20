제니퍼5 API 서버
---

제니퍼5 디비를 웹 API 로 조회할 수 있다.

#### 실행방법
```shell
cd 압축해제_경로/bin
./api-server or ./api-server.bat (for Windows)
```

#### 부가 설정 파라미터
```
-Dapi.server.host (기본값 0.0.0.0)  
-Dapi.server.port (기본값 8080)
```

#### 설정 파일 위치
```
압축해제_경로/conf/api_server.conf
```

---

*X-View 애플리케이션 통계*

`http://host:port/xview/statistic/application`

**API**

**XView 조회 세션**

*요청*  
```http request
GET http://host:port/transaction/session?domainId=1004&date=20200825
```
*응답*
```json
{ "sessionId":-1808865215487403966 } 
```

**XView 트랜잭션**

*요청*
```http request
GET http://host:port/transaction?sessionId=-1808865215487403966&format=[json or bytes]
```

*응답*
* json
```json
{
"transactions": [
  {
    "language": "PHP",
    "collectTime": 1592233201354,
    "oid": 75537,
    "businessIds": [],
    "elapsedTime": 180,
    "cpuTime": 176,
    "sqlCount": 42,
    "sqlTime": 4,
    "fetchCount": 1300,
    "fetchTime": 1,
    "externalCallCount": 0,
    "externalCallTime": 0,
    "id": 715720123591212115,
    "error": "",
    "guid": "",
    "clientId": 0,
    "ipAddress": "157.55.39.102",
    "userId": "",
    "applicationName": "[GET]jennifersoft.com/ko/jennifer-trial/",
    "browserName": "",
    "frontEndElapsedTime": 0,
    "networkTime": 0,
    "agentEndTime": 1592233201248,
    "jenniferFrontAppId": "",
    "jenniferFrontPageLoadId": ""
  }
],
"complete": true,
"searchingTime": 1592275980708
}
```
* bytes (using [FlatBuffer](https://github.com/google/flatbuffers))
```kotlin
val transactionBySession = TransactionBySession.getRootAsTransactionBySession(ByteBuffer.wrap(receivingBytes))
```

**텍스트**

*요청*
```http request
POST http://host:port/jennifer-text?domainId=1004&date=20200616

원하는 텍스트만 조회시 해당 키를 body parameter로 전송
--> { keys=1, 2, 3[...] }
```

*응답*
* bytes (using [FlatBuffer](https://github.com/google/flatbuffers))
```kotlin
val textBySession = TextBySession.getRootAsTextBySession(ByteBuffer.wrap(receivingBytes))
```

**애플리케이션 통계**

*요청*
```http request
GET http://host:port/application-statistic?domainId=1004&date=20200616
```

*응답*
* json
```json
[
   {
      "applicationNameKey":2066290246,
      "callCount":48,
      "responseTimeSum":5246,
      "cpuTimeSum":4713,
      "failureCount":0,
      "badResponseCount":0,
      "sqlCount":8688,
      "sqlTimeSum":586,
      "externalCallCount":0,
      "externalCallTimeSum":0,
      "fetchCount":11618,
      "fetchTimeSum":37,
      "frontEndMeasureCount":0,
      "frontEndDomTimeSum":0,
      "frontEndRenderTimeSum":0,
      "frontEndNetworkTimeSum":0,
      "maxResponseTime":152,
      "responseTimeSquareSumOrN1":576520
   },
   {
      "applicationNameKey":1626251908,
      "callCount":1,
      "responseTimeSum":171,
      "cpuTimeSum":169,
      "failureCount":0,
      "badResponseCount":0,
      "sqlCount":51,
      "sqlTimeSum":6,
      "externalCallCount":0,
      "externalCallTimeSum":0,
      "fetchCount":1304,
      "fetchTimeSum":2,
      "frontEndMeasureCount":0,
      "frontEndDomTimeSum":0,
      "frontEndRenderTimeSum":0,
      "frontEndNetworkTimeSum":0,
      "maxResponseTime":171,
      "responseTimeSquareSumOrN1":29241
   }
]
```

**매트릭**

*요청*
```http request
GET http://host:port/metric?domainId=1004&date=20200616&otype=system&oid=10000&metric=active_service
```

*응답*
* 1440 개의 소수점 엘리먼트를 갖는 json array

**조회 범위내 에이전트 목록**
```http request
GET http://host:port/domains/{domainId}/agentsOfDate?date=yyyyMMdd
```

*응답*
* json
```json
[
   {
      "instanceId":7102,
      "oid":72638
   },
   {
      "instanceId":7777,
      "oid":73313
   },
   {
      "instanceId":10001,
      "oid":75537
   },
   {
      "instanceId":10002,
      "oid":75538
   },
   {
      "instanceId":10009,
      "oid":75545
   }
]
```
