
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



