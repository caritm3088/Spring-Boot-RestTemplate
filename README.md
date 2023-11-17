# SpringBootRestTemplate
 Using RestTemplate in Spring Boot Applications

## 一、概述
Fork了一个RestTemplate项目，<https://github.com/lokeshgupta1981/Spring-Boot-RestTemplate>，在此基础上进行了修改，增加了ClientApp和WebApp之间压缩传输的处理。
  
ClientApp压缩请求消息的Body，WebApp解压缩请求消息的Body，再进行后续处理。

## 二、修改说明
### ClientApp部分
1. ClientApp增加了一个拦截器：GzipRequestInterceptor，用于压缩请求的Body。
2. RestTemplateConfig中增加了一行，做拦截器的设置。
3. User类做了简化。
### WebApp部分
1. WebApp的pom中h2增加了version。
2. 增加了一个过滤器：GZipServletFilter，用于对压缩的Body进行解压。
3. User类做了简化，对应的数据库脚本也做了修改。

## 三、运行说明
1. 下载代码
```
git clone https://github.com/caritm3088/Spring-Boot-RestTemplate.git
```
2. 运行项目

运行两个项目，相关地址如下：
* ClientApp：<http://localhost:8081/users>
* WebApp地址：<http://localhost:8081/users>
* WebApp内嵌数据库地址：<http://localhost:8081/h2>

3. 测试

用Postman或Apifox连接ClientApp测试，选POST；URL：<http://localhost:8080/users>；
Header填写：
Content-Type：application/json；
body中选择json，输入如下内容：
```json
{
  "id": 6,    
  "name": "6666666666",
  "email": "666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666@Doe"
}
```

ClientApp控制台输出如下：
```shell
#### Body Original Content: ####
{"id":6,"name":"6666666666","email":"666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666@Doe"}
#### Body Original Length: ####
133
#### Body Compressed Content: ####
��V�LQ�2�Q�K�MU�R2�%�����Aj��T�Z����
#### Body Compressed Length: ####
56
```
长度133的内容，压缩至56

WebApp控制台输出如下：
```shell
#### before decompress: #### 
56
#### content after decompress: #### 
{"id":6,"name":"6666666666","email":"666666666666666666666666666666666666666666666666666666666666666666666666666666666666666666@Doe"}
#### size after decompress: #### 
133
```
内容长度56，解压后长度133
最后，可以成功入库。