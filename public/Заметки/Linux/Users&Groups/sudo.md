sudo – просто утилита (никакой магии), которая помогает безопасно эскалировать права доступа.

Работает это через SUID:
```bash
$ ls -lh $(which sudo)
-rwsr-xr-x 1 root root 328K Jun 25  2025 /usr/bin/sudo
```
- При исполнении программы устанавливается effective UID root пользователя

## Настройка

Настройки sudo хранятся в `/etc/sudoers`
Редактировать только через:
```bash
visudo
```

Формат записей такой:
```
USER/GROUP HOSTS = (RUNAS) OPTIONS: COMMANDS
```

### USER/GROUP
Сначала указывается конкретный пользователь или группа.
Пользователь:
```
alex ALL = (...
```

Группа:
```
%dev ALL = (...
```
- Перед названием группы ставим %

### HOSTS
Тут обычно просто ALL.

### RUNAS
От чьего имени можно запускать:
```bash
sudo -u otheruser ...
```

Пример:
```
alex ALL = (root) ...
alex ALL = (root, user1) ...
alex ALL = (ALL) ...
```
- `ALL` – от имени любого пользователя

### COMMANDS
Какие команды можно запускать.
Пример:
```yml
# Все команды
alex ALL = (root, user1) ALL
# Все кроме bash
alex ALL = (root) ALL, !/bin/bash
# Только одну команду
alex ALL = (root) /usr/bin/passwd
# Ограничиваем аргумент
alex ALL = (root) /usr/bin/passwd user1
```

### OPTIONS
Теги, меняющие поведение sudo.
Пример:
```yml
# Все команды без пароля от имени root
alex ALL = (root) NOPASSWD: ALL
# Несколько опций и команд
alex ALL=(root) NOPASSWD:NOEXEC: /usr/bin/less PASSWD: /usr/bin/passwd 
```