Сохранить образ:
```bash
docker save example:latest | gzip > example_latest.tar.gz
```

Загрузить образ:
```bash
docker load < example_latest.tar.gz
```