
# 常见问题

## 验签问题

### 1.为什么提示缺少api_key（code为84006）？
请确保请求头中包含ACCESS-KEY

### 2.为什么提示缺少签名（code为84002）？
请确保请求头中包含ACCESS-SIGN

### 3.为什么提示缺少时间戳（code为84001）？
请确保请求头中包含ACCESS-TIMESTAMP

### 4.为什么签名认证总返回签名错误（code为84004）？
签名不正确导致的:
1)可以参考postman中实现，接口文件请先参考

2)如果是自己编写签名函数，请务必一步步地参照如下描述：

ACCESS-SIGN的请求头是对ACCESS-TIMESTAMP、ACCESS-KEY及请求参数，先按照参数名的字典顺序进行排序，然后依次用&字符拼接，生成待签名的字符串，最后根据SecretKey，使用HMAC SHA256方法加密，通过Base64编码输出而得到的;

检查时，可以打印出请求头信息和签名前字符串，重点有以下几点：
(1) 将您的请求头和下面的请求头示例一一对比，

请求头示例：
```
Content-Type: application/x-www-form-urlencoded

ACCESS-KEY: 09c2c831-6737-49db-879e-0b21416a54f6

ACCESS-SIGN: S5KQGc3tZt6AXgdJqVN7dTzyFRBVKyRAUrcDC+FzjA4=

ACCESS-TIMESTAMP: 1606727173506
```

(2) 是否在程序中正确地配置了APIKey

(3) 签名前字符串是否符合标准格式，所有要素按约定的协议保持一致，可以复制如下示例跟您的签名前字符串进行比对：

请求头：
```
ACCESS-KEY=09c2c831-6737-49db-879e-0b21416a54f6
ACCESS-TIMESTAMP=1543057116
```

请求参数：
```
symbol：tok_eth
type：1
entrustPrice： 680
entrustCount： 100
```

签名前字符串
```
ACCESS-KEY=09c2c831-6737-49db-879e-0b21416a54f6&ACCESS-TIMESTAMP=1543057116&entrustCount=100&entrustPrice=1.03&symbol=tok_eth&type=1
```

(4) 签名结果（ACCESS-SIGN）的长度应为44位，如：S5KQGc3tZt6AXgdJqVN7dTzyFRBVKyRAUrcDC+FzjA4=，如果长度为88位，是对签名结果进行了16进制编码导致。

## 限速问题

#### 1.API每秒调用频率有限制吗？

有限制，具体可以看下文档中每个接口的访问频率限制。

#### 2.API调用接口报超过访问频率会被封IP吗？封多久？

不会的，降低访问频率就可以。

#### 3.访问接口报错超频，怎么解决？

降低访问频率，文档的每个接口下方都有频率说明，需要将请求频率降低到频率说明之下。
