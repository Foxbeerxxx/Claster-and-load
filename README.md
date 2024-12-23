# Домашнее задание к занятию "`Кластеризация и балансировка нагрузки`" - `Татаринцев Алексей`



### Задание 1

`Начинаю делать все по лекции`

1. `Создаю директории http1 и http2`

![1](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img1.png)`

2. `Запустил два simple python сервера на своей виртуальной машине на разных портах`

![2](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img2.png)`

3. `Проверяю балансировку с весом 3 на первый сервер по седьмому уровню OCI`

![3](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img3.png)`

4. `Проверяем балансировку по 4 уровню`

![4](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img4.png)`

5. `Установил Haproxy  и настроил conf - файл:`

```
global
	log /dev/log	local0
	log /dev/log	local1 notice
	chroot /var/lib/haproxy
	stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
	stats timeout 30s
	user haproxy
	group haproxy
	daemon

	# Default SSL material locations
	ca-base /etc/ssl/certs
	crt-base /etc/ssl/private

	# See: https://ssl-config.mozilla.org/#server=haproxy&server-version=2.0.3&config=intermediate
        ssl-default-bind-ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384
        ssl-default-bind-ciphersuites TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
        ssl-default-bind-options ssl-min-ver TLSv1.2 no-tls-tickets

defaults
	log	global
	mode	http
	option	httplog
	option	dontlognull
        timeout connect 5000
        timeout client  50000
        timeout server  50000
	errorfile 400 /etc/haproxy/errors/400.http
	errorfile 403 /etc/haproxy/errors/403.http
	errorfile 408 /etc/haproxy/errors/408.http
	errorfile 500 /etc/haproxy/errors/500.http
	errorfile 502 /etc/haproxy/errors/502.http
	errorfile 503 /etc/haproxy/errors/503.http
	errorfile 504 /etc/haproxy/errors/504.http

listen stats  # веб-страница со статистикой
        bind                    :888
        mode                    http
        stats                   enable
        stats uri               /stats
        stats refresh           5s
        stats realm             Haproxy\ Statistics

frontend example  # секция фронтенд
        mode http
        bind :8088
        default_backend web_servers
	#acl ACL_example.com hdr(host) -i example.com
	#use_backend web_servers if ACL_example.com

backend web_servers    # секция бэкенд
       mode http
        balance roundrobin
        option httpchk
        option httpchk GET /index.html
        server s1 127.0.0.1:8888 check
        server s2 127.0.0.1:9999 check



listen web_tcp

	bind :1325

	server s1 127.0.0.1:8888 check inter 3s
	server s2 127.0.0.1:9999 check inter 3s
```



5. `Поправил, благодаря подсказке в статье от Александра Зубарева - Спасибо `
`Сервис заработал`

![7](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img7.png)`



### Задание 2



1. `Добавляю третий сервер, но перед этим создаю директорию http3`
2. `Запускаю простой Python сервер и проверяю`
![8](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img8.png)`

3. `Добавляю веса серверов в файл upstream.inc `
![9](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img9.png)`

4. `Меняем название сервера для обращения к ниму в файле example-http.conf`

![12](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img12.png)`

5. `Делаем проверку обращаясь к example.local`
![10](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img10.png)`

6. `Смотрим страницу статистики http://10.0.2.15:888/stats`
![13](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img13.png)`
 
`Конфигурационный файл haproxy прилагаю`



