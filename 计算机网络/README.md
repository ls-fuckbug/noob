# 浏览器发起一次HTTPS请求的过程详解

## 构建请求

## 查找浏览器缓存

## DNS解析

1. 查询浏览器DNS缓存

2. 查询操作系统DNS缓存

3. 读取hosts文件

4. 请求DNS服务器

## TCP三次握手

## TLS握手

### 客户端发送client Hello ,消息包含TLS版本以及client random随机字符串

### 服务端发送server Hello, 消息包含数字证书以及server random随机字符串

### 客户端验证证书

1. 检查数字签名

2. 验证证书链

3. 检查证书有效期

4. 检查证书的撤回状态

### 客户端发送 premaster secret字符串，利用服务端公钥加密 （加密算法如RSA)

### 服务端利用服务端私钥解密获得premaster secret

### 客户端和服务端利用client random 、server random、 premaster secret 生成共享密钥

### 客户端就绪，发送共享密钥加密过的finished

### 服务端就绪， 发送共享密钥加密过的finished

### 达成安全通信，通过共享密钥加密传输数据

### 四次挥手



