## Пример конфига
На клиенте лежит в `~/.ssh/config`:
```
Host proxy
	Hostname 185.193.102.77
	Port 20
	User alex
	IdentityFile ~/.ssh/id_vpn
	StrictHostKeyChecking accept-new
	ForwardAgent yes
	ServerAliveInterval 20

Host my-host
	Hostname 85.142.173.64
	User alex
	IdentityFile ~/.ssh/id_vova
	StrictHostKeyChecking no
	ServerAliveInterval 20
	ProxyJump proxy
```
### Опции
- `Hostname` – домен/IP-aдрес хоста. Если не указан, то идентичен значению `Host`
- `User` – пользователь на тачке. Если не указан, то используется пользователь с локалки клиента
- `Port` – порт ssh на сервере
- `IdentityFile` – путь до приватного ключа на клиенте
- `StrictHostKeyChecking` – политика проверки ключа сервера. Допустимы значения:
	- `no` – отключаем
	- `yes` – включаем (дефолт)
	- `accept-new` – принимаем новые хосты, но отказываем в подключении, если адрес поменялся
- `ForwardAgent` – прокинуть агент на хост
- `ServerAliveInterval` – кидать пакеты, что я живой, чтобы сервер не закрыл подключение
- `ProxyJump` – если хост не доступен напрямую по ssh, то можно сначала подключиться к машине, которая нам доступна, и с которой доступен целевой хост. Для пользователя это прозрачно, ssh все делает сам.

