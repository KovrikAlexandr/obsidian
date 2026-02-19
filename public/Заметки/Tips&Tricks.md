
Сохранить цепочку сертов в файл:
```bash
SERVER_NAME=example.com
PORT=443
OUTPUT="fullchain.pem"
openssl s_client \
-connect "$SERVER_NAME:$PORT" \
-servername "$SERVER_NAME" \
-showcerts </dev/null 2> /dev/null | \
awk '/-----BEGIN CERTIFICATE-----/ {p=1}; p; /-----END CERTIFICATE-----/ {p=0}' | sudo tee "$OUTPUT"
```

Вывести переменную ansible:
```bash
ansible sage[0] -m debug -a "var=sage_admin_password"
```

Скопировать образ между registry:
```bash
IMAGE='otel/opamp-server-migration:ded0b339' SRC='docker-hosted.artifactory.tcsbank.ru/sage' DST='sit-registry.m1.sage-next.local/sage' skopeo copy "docker://$SRC/$IMAGE" "docker://$DST/$IMAGE"
```

```bash
sudo skopeo copy \
"docker://docker-hosted.artifactory.tcsbank.ru/sage/otel/opamp-server:ded0b339" \
"docker://sit-registry.m1.sage-next.local/sage/otel/opamp-server:ded0b339"
```