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
        http-check send meth GET uri /index.html
        server s1 127.0.0.1:8888 check
        server s2 127.0.0.1:9999 check


listen web_tcp

	bind :1325

	server s1 127.0.0.1:8888 check inter 3s
	server s2 127.0.0.1:9999 check inter 3s
```



5. `Сохранил файл и делоаю sudo systemctl reload haproxy и получаю ошибку, почему? Потратил чполтора часа так и не выкружил проблему, нужна подстказка, делал все строго по лекции`
`Скриншоты ошибок`

![Проверка Conf](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img5.png)`

![6](https://github.com/Foxbeerxxx/Claster-and-load/blob/main/img/img6.png)`


















### Задание 2

`Будет но нужно победить сначало первое задание`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота 2](ссылка на скриншот 2)`


