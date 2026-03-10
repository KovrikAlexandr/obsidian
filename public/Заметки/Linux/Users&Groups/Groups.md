Группа – коллекция пользователей. Используется для управления правами доступа.

Информация хранится в:
- [/etc/group](obsidian://open?vault=obsidian&file=public%2F%D0%97%D0%B0%D0%BC%D0%B5%D1%82%D0%BA%D0%B8%2FLinux%2FUsers%26Groups%2Fgroup) – основная инфа
- [/etc/gshadow](obsidian://open?vault=obsidian&file=public%2F%D0%97%D0%B0%D0%BC%D0%B5%D1%82%D0%BA%D0%B8%2FLinux%2FUsers%26Groups%2Fgshadow) – пароль + админы (почти не используется)

## Primary group

Primary group – одна группа, привязанная к пользователю. Обычно создаётся вместе с юзером и называется так же.

Нужна, чтобы присваивать эту группу файлам, которые создаёт этот юзер.

## Secondary groups

Все остальные группы. В них юзер просто числится.

## Базовые операции

Создать:
```bash
sudo groupadd mygroup
```

Добавить юзера в новую группу (docker, например):
```bash
sudo usermod -aG mygroup alex
```

Удалить юзера из групп:
```bash
sudo usermod -rG mygroup alex
```

Удалить группу:
```bash
sudo groupdel mygroup
```