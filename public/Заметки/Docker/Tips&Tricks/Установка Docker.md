Docker не содержится в стандартных репозиториях, поэтому надо будет добавить репозиторий вручную. Итак по шагам.

## 0. Стандартный репозиторий
Для `apt` можно скачать `docker` можно из стандартных репозиториев. проблема в том, что там версия старая и лучше все таки качать из официального репозитория.

Из стандартного так:
```bash
sudo apt update
sudo apt install docker.io
```

Проверить:
```bash
docker -v
```

Из официального репозитория `docker` читать далее.
## 1. Установить нужные зависимости
Для этого хватит стандартного пакетного менеджера:
```bash
sudo apt update
sudo apt install -y \
apt-transport-https \
ca-certificates \
curl \
gnupg-agent \
software-properties-common
```
Зачем нужны эти пакеты:
- `apt-transport-https` – позволяет использовать репозитории по протоколу HTTPS
- `ca-certificates` – содержит сертификаты доверенных центров сертификации
- `curl` – для загрузки `gpg` ключа
- `gnupg-agent` – для работы с `gpg` ключами
- `software-properties-common` – позволяет управлять репозиториями.

## 2. Добавляем gpg-ключ репозитория
Перед этим может понадобится указать IP-адрес репозитория в `/etc/hosts`:
```
3.128.0.0 download.docker.com
```
Ключ нужен для проверки подлинности репозитория. Скачиваем ключ:
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
Флаги `curl`:
- **`-f`** — не показывать тело ошибки, если сервер вернул ошибку (тихий режим).
- **`-s`** — тихий режим (без индикаторов прогресса).
- **`-S`** — показывать ошибки, даже если `-s` включён.
- **`-L`** — следовать редиректам, если сервер перенаправляет на другой URL.

Команда `gpg --dearmor` конвертирует ключ из текстового формата, в котором он приходит в бинарный.
Флаг `-o` это файл, в который запишется ключ.

## 3. Добавление Docker-репозитория
Теперь можно добавить оффициальный docker-репозиторий в систему:
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Команда пишет в файл `/etc/apt/sources.list.d/docker.list` информацию о новом репизитории.

## 4. Установка пакетов Docker
Обновим информацию о пакетах и установим пакеты, требуемые для `docker`:
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

## 5. Запуск Docker
Проверяем установку (`sudo` права для этого не нужны):
```bash
docker --version
docker compose version
```

Если все ок, запускаем:
```bash
sudo systemctl start docker
sudo systemctl status docker
```

Включаем автозапуск при ребуте:
```bash
sudo systemctl enable docker
```

Чтобы запускать docker без sudo:
```bash
sudo usermod -aG docker ubuntu
newgrp docker
```

## 6. Удаление Docker

Удаляем docker:
```bash
sudo systemctl stop docker

# Удалите все контейнеры, образы, тома и сети
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd

# Удалите Docker Engine
sudo apt purge -y docker docker-engine docker.io containerd runc

# Удалите репозиторий Docker (если добавляли)
sudo rm -rf /etc/apt/sources.list.d/docker.list

# Удалите GPG-ключ Docker
sudo apt-key del 0EBFCD88 || true
```









