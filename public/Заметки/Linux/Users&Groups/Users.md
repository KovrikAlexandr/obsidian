Данные о пользователях хранятся в [/etc/passwd](obsidian://open?vault=obsidian&file=public%2F%D0%97%D0%B0%D0%BC%D0%B5%D1%82%D0%BA%D0%B8%2FLinux%2FUsers%2Fpasswd)
Данные о паролях хранятся в [/etc/shadow](obsidian://open?vault=obsidian&file=public%2F%D0%97%D0%B0%D0%BC%D0%B5%D1%82%D0%BA%D0%B8%2FLinux%2FUsers%2Fshadow)

## Базовые операции

Создать пользователя:
```bash
sudo useradd alex
```

Информация о пользователе и его группах:
```bash
id alex
```

Поменять пароль:
```bash
sudo passwd alex
# Enter new password
```

Добавить в новую группу (docker, например):
```bash
sudo usermod -aG docker alex
```

Удалить из групп:
```bash
sudo usermod -rG g1,g1 alex
```

Поменять базовые аттрибуты:
```bash
sudo usermod \
-l "balex" \
-d "/home/balex" --move-home \
-u 777 \ # UID
-g 888 \ # GID
-p "newpassword" \
-s "/bin/zsh" \
alex
```

Удалить пользователя:
```bash
sudo userdel alex
```
- `-f` – force. Удалить, даже, если пользователь залогинен
- `-r`– recursively. Удалить /home директорию
