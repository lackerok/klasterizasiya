# Задание 1


<img width="1086" height="203" alt="image" src="https://github.com/user-attachments/assets/5b8f2e17-50b4-4143-a519-7c6f041d2edf" />

<img width="1347" height="245" alt="image" src="https://github.com/user-attachments/assets/c394e6ca-967f-40fb-8c61-4e844740a2bb" />

```
global
    log /dev/log local0
    log /dev/log local1 notice
    chroot /var/lib/haproxy
    user haproxy
    group haproxy
    daemon

defaults
    log     global
    mode    tcp
    timeout connect 5000
    timeout client  50000
    timeout server  50000

listen stats
    mode http
    bind :888
    stats enable
    stats uri /stats
    stats refresh 5s

frontend web_frontend_l4
    bind :80
    mode tcp
    default_backend web_servers_l4

backend web_servers_l4
    mode tcp
    balance roundrobin
    server s1 127.0.0.1:8888 check
    server s2 127.0.0.1:9999 check
```

# Задание 2

Без домена:
<img width="1296" height="189" alt="image" src="https://github.com/user-attachments/assets/c411f92d-a47b-4d6b-a219-200ccf209da4" />

С доменом:
<img width="850" height="140" alt="image" src="https://github.com/user-attachments/assets/dc88ee9d-6979-4e30-96f5-221e196df410" />

<img width="1036" height="184" alt="image" src="https://github.com/user-attachments/assets/99dc0e96-7a04-40c3-a351-5e53f97d0ffb" />

<img width="1239" height="225" alt="image" src="https://github.com/user-attachments/assets/cf672a4e-3e8a-4628-a19a-28d5d7b3787b" />

```
global
    log /dev/log local0
    log /dev/log local1 notice
    chroot /var/lib/haproxy
    user haproxy
    group haproxy
    daemon

defaults
    log     global
    mode    http
    timeout connect 5000
    timeout client  50000
    timeout server  50000

listen stats
    mode http
    bind :888
    stats enable
    stats uri /stats
    stats refresh 5s

frontend web_frontend_l7
    bind :80
    mode http
    option httplog

    acl is_example_local hdr(host) -i example.local

    use_backend web_servers_l7 if is_example_local

backend web_servers_l7
    mode http
    balance roundrobin
    server s1 127.0.0.1:8888 weight 2 check
    server s2 127.0.0.1:9999 weight 3 check
    server s3 127.0.0.1:7777 weight 4 check
```
