#### nginx 的应用场景

  1. 作为静态资源的服务器

  2.  nginx 做反向代理

          ```xml

          upstream tomcat1 {
            server 192.168.25.148:8080;
              }
              server {
                  listen       80;
                  server_name  www.sina.com.cn;

                  #charset koi8-r;

                  #access_log  logs/host.access.log  main;

                  location / {
                      proxy_pass   http://tomcat1;
                      index  index.html index.htm;
                  }
              }

          ```

  3. nginx 做负载均衡

        ```xml

         upstream tomcat2 {
              server 192.168.25.148:8081;
              server 192.168.25.148:8082 weight=2;
                }        
            
        ```
  

  4. nginx +keepalived 实现高可用