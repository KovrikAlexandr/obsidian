
Посмотреть все теги образа в registry:
```bash
skopeo list-tags docker://sit-registry.m1.sage-next.local/sage/alerts-sentinel
```


Скопировать образ между registry:
```bash
sudo skopeo copy \
docker://docker-hosted.artifactory.tcsbank.ru/sage/mcc-migrations:release-93907019 \
docker://sit-registry.m1.sage-next.local/sage/mcc-migrations:release-93907019
```