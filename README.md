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
