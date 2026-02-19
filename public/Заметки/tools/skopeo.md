
Посмотреть все теги образа в registry:
```bash
skopeo list-tags docker://sit-registry.m1.sage-next.local/sage/alerts-sentinel
```


Скопировать образ между registry:
```bash
IMAGE='ui-external:release-av723' \
SRC='docker-hosted.artifactory.tcsbank.ru/sage' \
DST='sit-registry.m1.sage-next.local/sage' \
skopeo copy "docker://$SRC/$IMAGE" "docker://$DST/$IMAGE"
```