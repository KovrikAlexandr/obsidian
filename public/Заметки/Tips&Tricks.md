
Сохранить цепочку сертов в файл:
```bash
SERVER_NAME=localhost
PORT=8006
OUTPUT="fullchain.pem"
openssl s_client \
-connect "$SERVER_NAME:$PORT" \
-servername "$SERVER_NAME" \
-showcerts </dev/null 2> /dev/null | \
awk '/-----BEGIN CERTIFICATE-----/ {p=1}; p; /-----END CERTIFICATE-----/ {p=0}' | sudo tee "$OUTPUT"
```

Вывести переменную ansible:
```bash
ansible logs_collector_api[0] -m debug -a "var=sage_spirit_external_domain"
```

Скопировать образ между registry:
```bash
export IMAGE='sage-trukk:release-av36' 
export DST='docker-hosted.artifactory.tcsbank.ru/sage' 
export SRC='sage-artifactory.tcsbank.ru/sage' 
skopeo copy "docker://$SRC/$IMAGE" "docker://$DST/$IMAGE"
```

```bash
sudo skopeo copy \
"docker://docker-hosted.artifactory.tcsbank.ru/sage/otel/opamp-server:ded0b339" \
"docker://sit-registry.m1.sage-next.local/sage/otel/opamp-server:ded0b339"
```
