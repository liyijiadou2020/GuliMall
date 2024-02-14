# GuliMall
The gulimall (Guli Mall) project is a set of e-commerce projects, including a front-end mall system and a back-end management system, implemented based on SpringCloud + SpringCloudAlibaba + MyBatis-Plus, and deployed using Docker containerization. The front-end mall system includes: user login, registration, product search, product details, shopping cart, order process, flash sale activities and other modules. The backend management system includes seven modules: system management, product system, preferential marketing, inventory system, order system, user system, and content management.

### Organizational structure

```java
gulimall project （gulimall.com）
├── gulimall-common - Utils and general code
├── gulimall-admin –  Backend management service （admin.gulimall.com）
├── gulimall-generator – Code-behind generator
├── gulimall-auth-server -- Certification Center （auth.gulimall.com）
├── gulimall-cart -- Shopping cart service（cart.gulimall.com）
├── gulimall-coupon -- Coupon service
├── gulimall-gateway -- Unified configuration gateway
├── gulimall-order -- Order service（order.gulimall.com）
├── gulimall-product -- Goods and Services（item.gulimall.com）
├── gulimall-search --  Search service（search.gulimall.com）
├── gulimall-seckill -- sckill service （seckill.gulimall.com）
├── gulimall-third-party -- Third-party services (OSS object cloud storage, SMS cloud text messaging)
├── gulimall-ware -- Warehousing Services
└── gulimall-member -- membership service （member.gulimall.com）

```

### Building steps

##### All resources are in the resource directory

##### The database is in the resource/db directory

##### Nginx static files are in the resource/static directory

##### Notes and courseware are in Guli Mall (including code, courseware, sql)

> Windows environment deployment


```
192.168.120.20 gulimall.com
192.168.120.20	gulimall.com
192.168.120.20	search.gulimall.com
192.168.120.20  item.gulimall.com
192.168.120.20  auth.gulimall.com
192.168.120.20  cart.gulimall.com
192.168.120.20  order.gulimall.com
192.168.120.20  member.gulimall.com
192.168.120.20  seckill.gulimall.com
192.168.120.20  seckill.gulimall.com
127.0.0.1  admin.gulimall.com
127.0.0.1 sso.com
127.0.0.1 client1.com
127.0.0.1 client2.com
```


```
1、在nginx.conf中添加负载均衡的配置    
upstream gulimall{
        server 192.168.120.1:88;
    }
2、在gulimall.conf中添加如下配置
server {
    listen       80;
    listen  [::]:80;
    server_name  gulimall.com  *.gulimall.com;

    #charset koi8-r;
    #access_log  /var/log/nginx/log/host.access.log  main;

    #access_log  /var/log/nginx/host.access.log  main;

    #配置静态资源的动态分离
    location /static/ {
        root   /usr/share/nginx/html;
    }

#支付异步回调的一个配置
    location /payed/ {
        proxy_set_header Host order.gulimall.com;        #不让请求头丢失
        proxy_pass http://gulimall;
    }

    location / {
        proxy_set_header Host $host;
        proxy_pass http://gulimall;
    }


    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}


```


### Technology selection

**Backend Technology**

|        Technology        |           Specification           |                      Site                       |
| :----------------: | :----------------------: | :---------------------------------------------: |
|     SpringBoot     |Container + MVC framework|     https://spring.io/projects/spring-boot      |
|    SpringCloud     |         Microservice architecture|     https://spring.io/projects/spring-cloud     |
| SpringCloudAlibaba |         a series of components| https://spring.io/projects/spring-cloud-alibaba |
|    MyBatis-Plus    |          ORM framework|             https://mp.baomidou.com             |
|  renren-generator  |  Code generator for Renren open source projects|   https://gitee.com/renrenio/renren-generator   |
|   Elasticsearch    |          search engine         |    https://github.com/elastic/elasticsearch     |
|      RabbitMQ      |          message queue         |            https://www.rabbitmq.com             |
|   Springsession    |         Distributed cache|    https://projects.spring.io/spring-session    |
|      Redisson      |          Distributed lock|      https://github.com/redisson/redisson       |
|       Docker       |        application container engine|             https://www.docker.com              |
|        OSS         |         Object cloud storage|  https://github.com/aliyun/aliyun-oss-java-sdk  |

**Front-end technology**

|        Technology        |           Specification       |                      Site                       |
| :-------: | :--------: | :-----------------------: |
|    Vue    |front-end framework|     https://vuejs.org     |
|  Element  |  Front-end UI framework| https://element.eleme.io  |
| thymeleaf |   template engine| https://www.thymeleaf.org |
|  node.js  |  Server side js|   https://nodejs.org/en   |

### Architecture diagram

**System Architecture Diagram**

[![UUvRAS.png](https://images.gitee.com/uploads/images/2020/0714/193425_4a1056c4_4914148.png)](https://imgchr.com/i/UUvRAS)

**Business Architecture Diagram**

![UUvb7T.png](https://images.gitee.com/uploads/images/2020/0714/193425_9bb153d1_4914148.png)

### Environment setup

#### Development tools

|     Tool      |        Specifications        |                      Sites                       |
| :-----------: | :-----------------: | :---------------------------------------------: |
|     IDEA      |Develop Java programs |     https://www.jetbrains.com/idea/download     |
| RedisDesktop  |  redis client connection tool |        https://redisdesktop.com/download        |
|  SwitchHosts  |     Local host management |       https://oldj.github.io/SwitchHosts        |
|    X-shell    |   Linux remote connection tool | http://www.netsarang.com/download/software.html |
|    Navicat    |    Database connection tool |       http://www.formysql.com/xiazai.html       |
| PowerDesigner |    Database design tools |             http://powerdesigner.de             |
|    Postman    |    API interface debugging tool |             https://www.postman.com             |
|    Jmeter     |     Performance stress testing tools |            https://jmeter.apache.org            |
|    Typora     |    Markdown editor |                https://typora.io                |

#### Development environment

|     Sofeware      | Edition |                             Site                             |
| :-----------: | :----: | :----------------------------------------------------------: |
|      JDK      |  1.8   | https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html |
|     Mysql     |  5.7   |                    https://www.mysql.com                     |
|     Redis     | Redis  |                  https://redis.io/download                   |
| Elasticsearch | 7.6.2  |               https://www.elastic.co/downloads               |
|    Kibana     | 7.6.2  |               https://www.elastic.co/cn/kibana               |
|   RabbitMQ    | 3.8.5  |            http://www.rabbitmq.com/download.html             |
|     Nginx     | 1.1.6  |              http://nginx.org/en/download.html               |




