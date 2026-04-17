Nginx – веб сервер

Есть главный процесс – master process. Он обрабатывает сигналы, читает файлы конфигурации и управляет worker процессами.

Worker process – процесс, который обрабатывает запросы.

Основная конфигурация nginx хранится в файле `nginx.conf` в каталогах:
- `/etc/nginx/`
- `/usr/local/nginx/conf/`
- `/usr/local/etc/nginx/`

Отравка сигналов:
```bash
nginx -s SIGNAL
```

Сигналы:
- `stop` – быстрая остановка
- `quit` – качественное завершение
- `reload` – перечитать конфиг
- `reopen` – что-то с логами

PID master процесса можно посмотреть в файле `nginx.pid` в каталогах:
- `/var/run/`
- `/usr/local/nginx/logs/`

## Структура конфигов

Основной конфиг – `/etc/nginx/nginx.conf`

Обычно он содержит include-ы для других конфигураций:
```
http {
	include /etc/nginx/mime.types;
	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
}
```

В свою очередь файлы из `/etc/nginx/sites-enabled/` являются символьными ссылками на файлы в `/etc/nginx/sites-available/`. Таким образом можно описать конфигурацию в `sites-available`, после чего управлять ей через добавление/удаление симлинка в `sites-enabled`.