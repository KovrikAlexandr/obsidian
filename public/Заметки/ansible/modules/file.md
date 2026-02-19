Позволяет управлять:
- Файлами
- Директориями
- Символьными ссылками

## Примеры

Удалить директорию:
```yml
- name: Remove directory
  file:
	path: '/path/to/dir'
	state: 'absent'
```