Апстрим – бекенд НА который nginx пересылает трафик.

Пример в конфиге:
```
upstream mage-http {
    server dev0-apps-0.ds.sage-next.local:8004 max_fails=5 fail_timeout=10s;
    server dev0-apps-2.ds.sage-next.local:8004 max_fails=5 fail_timeout=10s;
    keepalive 16;
}
```

В location можно настроить обращение к апстриму:

- `proxy_connect_timeout` – таймаут на TCP подключение

- `proxy_read_timeout` – на TCP уровне всё окей, бекенд прочитал данные, но не отвечает на http запрос.

- `proxy_next_upstream_timeout` – общее время, за которое должен был выполнен запрос. Если оно превышается, Nginx отдаёт 504 (gateway timeout)

- `proxy_next_upstream_tries` – общее количество попыток достучаться до апстрима

- `proxy_next_upstream` – стратегия выбора следующего апстрима. Можно указать случаи, в которых мы хотим, чтобы был выбран другой апстрим

