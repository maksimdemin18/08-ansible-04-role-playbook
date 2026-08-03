## Домашнее задание к занятию "`Работа с roles`" - `Дёмин Максим`

### Задание 1
Подготовка к выполнению
Необязательно. Познакомьтесь с LightHouse.
Создайте два пустых публичных репозитория в любом своём проекте: vector-role и lighthouse-role.
Добавьте публичную часть своего ключа к своему профилю на GitHub.
Основная часть
Ваша цель — разбить ваш playbook на отдельные roles.

Задача — сделать roles для ClickHouse, Vector и LightHouse и написать playbook для использования этих ролей.

Ожидаемый результат — существуют три ваших репозитория: два с roles и один с playbook.

Что нужно сделать

Создайте в старой версии playbook файл requirements.yml и заполните его содержимым:

---
  - src: git@github.com:AlexeySetevoi/ansible-clickhouse.git
    scm: git
    version: "1.13"
    name: clickhouse 
При помощи ansible-galaxy скачайте себе эту роль.

Создайте новый каталог с ролью при помощи ansible-galaxy role init vector-role.

На основе tasks из старого playbook заполните новую role. Разнесите переменные между vars и default.

Перенести нужные шаблоны конфигов в templates.

Опишите в README.md обе роли и их параметры. Пример качественной документации ansible role по ссылке.

Повторите шаги 3–6 для LightHouse. Помните, что одна роль должна настраивать один продукт.

Выложите все roles в репозитории. Проставьте теги, используя семантическую нумерацию. Добавьте roles в requirements.yml в playbook.

Переработайте playbook на использование roles. Не забудьте про зависимости LightHouse и возможности совмещения roles с tasks.

Выложите playbook в репозиторий.

В ответе дайте ссылки на оба репозитория с roles и одну ссылку на репозиторий с playbook.



Исходный монолитный playbook разделён на три самостоятельные роли:

1. `clickhouse` — внешняя роль версии `1.13`;
2. `vector_role` — авторская роль установки и настройки Vector;
3. `lighthouse_role` — авторская роль развёртывания LightHouse.

```
ansible-galaxy role install -r requirements.yml -p roles --force
zsh: correct 'role' to 'roles' [nyae]? n
Starting galaxy role install process
- changing role clickhouse from 1.13 to 1.13
- extracting clickhouse to /home/maksimd/Disk_DD/netology/devops_all/013_ansible/08-ansible-04-role-solution/08-ansible-04-role-playbook/roles/clickhouse
- clickhouse (1.13) was installed successfully
- extracting vector_role to /home/maksimd/Disk_DD/netology/devops_all/013_ansible/08-ansible-04-role-solution/08-ansible-04-role-playbook/roles/vector_role
- vector_role (1.0.0) was installed successfully
- extracting lighthouse_role to /home/maksimd/Disk_DD/netology/devops_all/013_ansible/08-ansible-04-role-solution/08-ansible-04-role-playbook/roles/lighthouse_role
- lighthouse_role (1.0.0) was installed successfully

```

Репозиторий содержит playbook, который устанавливает роли через `ansible-galaxy`, создаёт базу и таблицу ClickHouse дополнительными задачами, настраивает Vector для отправки событий в ClickHouse и публикует LightHouse через nginx.

## Структура

```
.
├── ansible.cfg
├── group_vars
│   ├── all.yml
│   ├── clickhouse.yml
│   ├── lighthouse.yml
│   └── vector.yml
├── inventory
│   └── prod.yml
├── requirements.yml
├── site.yml
└── SOLUTION.md
```

## Установка ролей

```
ansible-galaxy role install -r requirements.yml -p roles --force
```

## Подготовка inventory

Адреса `192.0.2.10`, `192.0.2.20` и `192.0.2.30` относятся к документальному диапазону и должны быть заменены реальными адресами управляемых узлов. При необходимости следует изменить пользователя и путь к ключу в `group_vars/all.yml`.

## Проверка синтаксиса

```
ansible-playbook --syntax-check site.yml
```

## Запуск

```
ansible-playbook site.yml
```

После выполнения:

- ClickHouse принимает HTTP-запросы на порту `8123`;
- база `logs` содержит таблицу `logs`;
- Vector формирует демонстрационные syslog-события и записывает нормализованные поля в ClickHouse;
- LightHouse доступен по адресу `http://<lighthouse-host>:8080/`.

## Проверка результата

```
curl -sS 'http://<clickhouse-host>:8123/?query=SELECT%20count()%20FROM%20logs.logs'
curl -I 'http://<lighthouse-host>:8080/'
```
