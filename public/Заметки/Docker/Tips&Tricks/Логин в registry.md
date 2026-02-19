Креды сохраняем в файл `auth.json`. Пример для YC registry:
```json
{
  "created_at": "",
  "description": "",
  "encrypted_private_key": null,
  "format": "PEM_FILE",
  "id": "",
  "key_algorithm": "RSA_2048",
  "key_fingerprint": null,
  "pgp_key": null,
  "private_key": "",
  "public_key": "",
  "service_account_id": "ajevb6hcmo4vsj1u51lb"
}
```

Логинимся:
```bash
cat admin.json | sudo docker login --username json_key --password-stdin cr.yandex
```

Разлогиниться:
```bash
sudo docker logout
```
