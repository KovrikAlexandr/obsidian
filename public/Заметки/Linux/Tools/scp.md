Можно меняться файлами между хостами по `ssh` с помощью `scp`.

## Утилита SCP
Утилита использует `ssh` для обмена файлами между разными хостами.
Загрузить файл с локалки на удалённый хост:
```bash
scp local-file user@host:/path/to/remote-file
```

Скачать файл с удаленного хоста на локалку:
```bash
scp user@host:/path/to/remote-file /path/to/local-file
```

Можно указать флаг `-r`, чтобы передать директорию:
```bash
scp -r local-dir user@host:/path/to/remote-dir
```
Аналогично в другую сторону.

```bash
sudo scp -rvC -i ~/sage-ssh/sit_poligon.id_rsa step/ .ara/ clans/ sage-ssh/ certificates/ sage@prod-control-plane-1.ru-central1.internal:/home/sage/polygon-install/
```

```bash
sudo scp -rC -i ~/sage-ssh/sit_poligon.id_rsa ./* sage@prod-control-plane-1.ru-central1.internal:/home/sage/polygon-install/
```



