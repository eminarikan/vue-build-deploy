## Apache Http Server
- vue projesinin buildine `.htaccess` adlı bir dosya eklenir ve içerisine:
  ```
  <IfModule mod_negotiation.c>
    Options -MultiViews
  </IfModule>

  <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule ^index\.html$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.html [L]
  </IfModule>
  ```
  eklenir.
- wampserver için:
  * proje buildi `wamp/www` konumuna yerleştirilir
  * `/wamp/bin/apache/apachexxx/conf/httpd.conf` içerisindeki `# Include conf/extra/httpd-vhosts.conf
` satırı yorumdaysa yorumdan çıkarılır.
  * `/wamp/bin/apache/apachexxx/conf/extra/httpd-vhosts.conf` içerisindeki `DocumentRoot` ve `Directory` düzeltilir.
  ```
  <VirtualHost *:80>
   DocumentRoot "d:/wamp/www/wp-one"
   ServerName wp-one
   <Directory "d:/wamp/www/wp-one">
      AllowOverride All
      Require local
   </Directory>
  </VirtualHost>
  ```
## Ngnix
- vue projesinin buildi `nginx/html` konumuna yerleştirilir.
- `ngnix/conf/ngix.conf` dosyasındaki `http.server.location` değişkeni;

  ```
    location / {
      try_files $uri $uri/ /index.html;
    }

  ```

  şeklinde değiştirilir.

## Tomcat
- vue projesinin buildi `tomcat/webapps/ROOT` konumuna yerleştirilir.
- `tomcat/conf/server.xml` dosyasındaki

  ```
  <Host name="localhost"  appBase="webapps"
              unpackWARs="true" autoDeploy="true">

      <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
          prefix="localhost_access_log" suffix=".txt"
          pattern="%h %l %u %t &quot;%r&quot; %s %b" />

  </Host>
  ```

  etiketine

  ```
   <Valve className="org.apache.catalina.valves.rewrite.RewriteValve" />
  ```

  satırı eklenir. Son Hali:

  ```
  <Host name="localhost"  appBase="webapps"
              unpackWARs="true" autoDeploy="true">

      <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
          prefix="localhost_access_log" suffix=".txt"
          pattern="%h %l %u %t &quot;%r&quot; %s %b" />

      <Valve className="org.apache.catalina.valves.rewrite.RewriteValve" />

  </Host>
  ```

- `tomcat/conf/Catalina/localhost` konumuna `rewrite.config` adlı bir dosya oluşturulur. Dosya içerisine:

  ```
  RewriteCond %{REQUEST_PATH} !-f
  RewriteRule ^/(.*) /index.html
  ```

  yazılır.
