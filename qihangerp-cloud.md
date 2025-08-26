## [qihangerp-cloud](https://github.com/zeasin/qihangerp-cloud)  has a backend SQL injection vulnerability

There is SQL injection when ${params.dataScope} is used in multiple places in the mybatis configuration file:

![image-20250826224722156](./images/image-20250826224722156.png)

Classes & Methods：SysRoleController#list

Path：/system/role/list

Directly query the incoming parameters：

![image-20250826225140089](./images/image-20250826225140089.png)

Time Blind Injection Test：

```
?pageNum=1&pageSize=10&status=0&params[dataScope]=AND+(select+sleep(5))--+
```

![image-20250826224734750](./images/image-20250826224734750.png)

Test successful ! 

