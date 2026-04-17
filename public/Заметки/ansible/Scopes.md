
Scope – пространство переменных. В зависимости от этого пространства переменная может быть определена/неопределена, иметь или не иметь значение

## Основные scopes

В Ansible есть три основных scope-а:
- Global scope
- Play scope
- Host scope
#### Global scope

Доступны везде.
Время жизни – весь плейбук.

Задать можно так:

1. Через запуск плейбука:

	```bash
	ansible-playbook setup.yml -e "global_var='global_value'" 
	```

2. Через env

3. Через ansible.cfg

#### Play scope

Доступна 

#### Host scope

Доступна для конкретного хоста из inventory. 
Время жизни – весь плейбук.

Задать можно:

1. Через инвентарь (точно ли это host scope ?):

	```yml
	backend:
		hosts:
			host-1.example.com
		vars:
			backend_port: 8080
	```

2. Через `group_vars/backend.yml`, если хост находится в этой группе:
	```yml
	backend_port: 8080
	```

3. Через `register`:

	```yml
	- command: echo hello  
	  register: result
	```

4. Через `set_fact`:

	```yml
	- set_fact:
	  my_var: "my_value"
	```
	Переменная буквально устанавливается в факты хоста
