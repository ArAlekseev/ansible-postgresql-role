# Ansible-роль для PostgreSQL

## Описание проекта

Домашнее задание по DevOps: разработка Ansible-роли для автоматической установки и настройки PostgreSQL на Ubuntu.

Роль выполняет установку PostgreSQL из стандартных репозиториев Ubuntu, инициализацию кластера в отдельном каталоге /data/postgres/data, настройку конфигурационных файлов и systemd-сервиса.

Также реализовано автоматическое создание баз данных и пользователей PostgreSQL, хранение паролей с помощью Ansible Vault и установка postgres-exporter для мониторинга базы данных.

Повторный запуск плейбука завершается с changed=0.

**Проект имеет следующую структуру:**

```text.
├── ansible.cfg
├── .gitignore
├── inventories
│   └── main
│       ├── group_vars
│       │   └── all
│       │       └── encrypted.yml
│       ├── hosts.yaml
│       └── host_vars
├── postgres.play.yml
├── README.md
└── roles
    └── postgres
        ├── defaults
        │   └── main.yml
        ├── files
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── README.md
        ├── tasks
        │   ├── exporter.yml
        │   └── main.yml
        ├── templates
        │   ├── pg_hba.conf.j2
        │   ├── postgres-exporter.env.j2
        │   ├── postgres-exporter.service.j2
        │   └── postgres.service.j2
        ├── tests
        │   ├── inventory
        │   └── test.yml
        └── vars
            └── main.yml
'''
```
**Основные файлы проекта:**

- `ansible.cfg` - конфигурация Ansible.
- `postgres.play.yml` - плейбук для запуска роли.
- `inventories/main/hosts.yaml` - inventory с целевым хостом `vm1`.
- `inventories/main/group_vars/all/encrypted.yml` - зашифрованные пароли пользователей PostgreSQL.
- `roles/postgres/defaults/main.yml` - переменные роли со значениями по умолчанию.
- `roles/postgres/tasks/main.yml` - задачи установки и настройки PostgreSQL.
- `roles/postgres/tasks/exporter.yml` - задачи установки postgres-exporter.
- `roles/postgres/templates/` - шаблоны конфигурационных файлов и systemd-сервисов.
- `roles/postgres/handlers/main.yml` - обработчики перезапуска служб.

**Переменные роли**

Основные переменные находятся в `roles/postgres/defaults/main.yml`:

- `postgres__data_dir` — путь к каталогу данных PostgreSQL.
- `postgres__config` — настройки PostgreSQL.
- `postgres__hba_rules` — правила подключения и аутентификации.
- `postgres__databases` — список создаваемых баз данных.
- `postgres__users` — список создаваемых пользователей.

Пароли пользователей хранятся в Ansible Vault (`encrypted.yml`).

**Запуск**

Для запуска необходимо установить Ansible и указать пароль от Ansible Vault в файле `vault-pass`.

Затем в терминале выполнить: ansible-playbook postgres.play.yml
