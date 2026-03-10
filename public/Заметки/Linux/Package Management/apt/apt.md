`apt` – утилита для работы с репозиториями и пакетами Ubuntu/Debian

## Основные команды

Обновить индексы (метаинформацию о пакетах):
```bash
sudo apt update
```
Подробнее про [Update](obsidian://open?vault=obsidian&file=public%2F%D0%97%D0%B0%D0%BC%D0%B5%D1%82%D0%BA%D0%B8%2FLinux%2FPackage%20Management%2Fapt%2FUpdate)

Установить пакет:
```bash
sudo apt install htop
```
- Информация о пакете получается из локального кеша
- Ищутся и скачиваются зависимости (описаны в метаданных)
- Скачивается и устанавливается сам пакет (силами `dpkg`)

Удалить пакет:
```bash
sudo apt remove htop
sudo apt purge htop
```
- `remove` – удаляет сам бинарь, но оставляет конфиги
- `purge` – удаляет все файлы, включая конфиги из `/etc/*`

Обновление пакетов:


