# Clova API 분석 (WIP)

## References

- [https://api-fin.ncloud-docs.com/docs/ai-application-service-ocr](https://api-fin.ncloud-docs.com/docs/ai-application-service-ocr)
- [https://api-fin.ncloud-docs.com/docs/ai-application-service-ocr-ocr](https://api-fin.ncloud-docs.com/docs/ai-application-service-ocr-ocr)
- [https://www.edwith.org/nclouddevtools/lecture/202788](https://www.edwith.org/nclouddevtools/lecture/202788)

# Clova Document OCR 인증 정보

- IDCard (주민등록증, 운전면허증, 여권)
    - endpoint : [https://au7h16ruau.apigw.ntruss.com/custom/v1/21390/70aa38b11f2146e78d93db4bbeab8457bc4497e9b4e417e9af0c6dec4ebad37d/document/id-card](https://au7h16ruau.apigw.ntruss.com/custom/v1/21390/70aa38b11f2146e78d93db4bbeab8457bc4497e9b4e417e9af0c6dec4ebad37d/document/id-card)
    - apikey(secret) : c0xBRFhEYlFhcXRReExJYkNOVkRadVBMV0Z5VkV4UXM=
- 사업자등록증
    - endpoint : [https://au7h16ruau.apigw.ntruss.com/custom/v1/21389/5072b840a752234d694303565cb0e2b524a6a15a60450fefd74e770c7a7ba456/document/biz-license](https://au7h16ruau.apigw.ntruss.com/custom/v1/21389/5072b840a752234d694303565cb0e2b524a6a15a60450fefd74e770c7a7ba456/document/biz-license)
    - apikey(secret) : eXZKdWZOUWxKanVMeHlDeFh5eXFQVnhwZUxkY3F6clc=

# 요청

- POST : CLOVA OCR 빌더에서 생성된 API Gateway의 InvokeURL로 호출

각 도메인마다 고유의 호출 URL이 생성됨

- TEXT OCR : 템플릿 정의없이 이미지의 모든 텍스트를 인식함 
- Path → /general
- Template OCR : 도메인에 배포된 템플릿이 포함된 이미지를 인식함 
- Path → /infer

# 요청 헤더

- X-OCR-SECRET : 도메인에서 API Gateway 연동 시 생성한 X-OCR-SECRET:{Client Secret}
- Content-Type 
- application/json : request use json body 
- multipart/form-data : request use multipart form body

# 요청 바디

```json
{
	"version": "string",
	"requestId": "string",
	"timestamp": 0,
	"lang": "string",
	"images": [{ "format": "string", "url": "string", "data": "string", "name": "string", "templateIds": [ 0 ] }],
	"enableTableDetection": true
}
```

- version : V1 또는 V2(권장) 입력
- requestId : API 호출 UUID
- timestamp : API 호출  Timestamp 값
- lang : OCR 인식 시 요청할 언어 정보 (’ko’ / ‘ja’ / ‘zh-TW’)
- images : Json array로 입력됨.
- images.format : 이미지 포맷을 설정 (”jpg”, “jpeg”, “png”, “pdf”, “tiff”)
- images.url / [images.data](http://images.data) 중 하나가 존재해야함.
- [images.name](http://images.name) : 이미지 명 입력
- images.templateIds : 텍스트 OCR API에서는 사용되지 않고 Template OCR API에서만 사용
- enableTableDetection : 문서 내 표(Table)영역을 자동 인식 후 글자와 함께 구조화된 형태로 제공

## 응답 바디(V2)

1. Image recognition Results 

| Image Recognition Results | {"uid": "string","name": "string","inferResult": "string","message": "string","matchedTemplate": { "id": 0, "name": "string" },"title": {},"fields": [ {} ],"validationResult": { "result": "string", "message": "string"}} |
| --- | --- |

1. Image Field object 

| Image Field | {"name": "string","valueType": "string","inferText": "string","inferConfidence": 0.0,"boundingPoly": {},"type": "string","subFields": [{}],"checked":true,"lineBreak":true} |
| --- | --- |
1. bounding Poly object 

| boundingPoly | {"vertices":[{}]} |
| --- | --- |
1. vertices object

| vertices | {"x": 0.0,"y": 0.0} |
| --- | --- |
1. SubField Object 

| subField | {"boundingPoly": {},"inferText": "string","inferConfidence":0.0"lineBreak": false}  |
| --- | --- |
1. ImageTable Object

| subField | {"cells": [],"boundingPoly": {},"inferConfidence":0.0} |
| --- | --- |
1. TableCell Object 

| subField | {"cellTextLines": [],"boundingPoly": {},"inferConfidence":0.0,"rowSpan":0,"rowIndex":0,"columnSpan":0,"columnIndex":0} |
| --- | --- |
1. CellTextLine Object 

| subField | {"cellWords": [],"boundingPoly": {},"inferConfidence":0.0} |
| --- | --- |
1. CellWord Object 

| subField | {"inferText": "","boundingPoly": {},"inferConfidence":0.0} |
| --- | --- |

## 예시 : TEXT OCR 요청

```
{
  "version": "V2",
  "requestId": "string",
  "timestamp": 0,
  "lang":"ko",
  "images": [
    {
      "format": "jpg",
      "name": "test 1",
      "url": "https://www.ncloud.com/ocr-test/test.jpg"
    }
  ]
}
```

## 예시 : TEXT OCR 응답

```
{
    "version": "V2",
    "requestId": "string",
    "timestamp": 1576569034247,
    "images": [{
        "uid": "9fd73a6aacad4025b3099a36ee55aacd",
        "name": "medium",
        "inferResult": "SUCCESS",
        "message": "SUCCESS",
        "fields": [{
                "valueType": "ALL",
                "inferText": "아름다운",
                "inferConfidence": 0.99992156,
                "type": "NORMAL",
                "lineBreak": true,
                "boundingPoly": {
                    "vertices": [{
                        "x": 2713.7295,
                        "y": 1277.0492
                    }, {
                        "x": 2713.7295,
                        "y": 977.7408
                    }, {
                        "x": 2841.4343,
                        "y": 977.7408
                    }, {
                        "x": 2841.4343,
                        "y": 1277.0492
                    }]
                }
            },
            {
                "valueType": "ALL",
                "inferText": "이",
                "inferConfidence": 0.99958915,
                "type": "NORMAL",
                "lineBreak": false,
                "boundingPoly": {
                    "vertices": [{
                        "x": 2314.6516,
                        "y": 1468.6066
                    }, {
                        "x": 2314.6516,
                        "y": 1328.9293
                    }, {
                        "x": 2426.3936,
                        "y": 1328.9293
                    }, {
                        "x": 2426.3936,
                        "y": 1468.6066
                    }]
                }
            },
            {
                "valueType": "ALL",
                "inferText": "세상",
                "inferConfidence": 0.9998707,
                "type": "NORMAL",
                "lineBreak": false,
                "boundingPoly": {
                    "vertices": [{
                        "x": 2314.6516,
                        "y": 1604.2931
                    }, {
                        "x": 2314.6516,
                        "y": 1460.625
                    }, {
                        "x": 2430.3843,
                        "y": 1460.625
                    }, {
                        "x": 2430.3843,
                        "y": 1604.2931
                    }]
                }
            }
        ],
        "validationResult": {
            "result": "NO_REQUESTED"
        }
    }]
}

```