# Решение домашнего задания «Работа с roles»

## Выполненные действия

1. Создан `requirements.yml` с ролью ClickHouse `AlexeySetevoi/ansible-clickhouse` версии `1.13`.
2. Подготовлена отдельная роль `vector-role` со структурой Ansible Galaxy.
3. Задачи, пользовательские переменные, внутренние переменные, шаблоны и обработчики Vector распределены по соответствующим каталогам роли.
4. Подготовлена отдельная роль `lighthouse-role`, устанавливающая LightHouse и формирующая конфигурацию nginx.
5. Для обеих ролей составлена документация с требованиями, параметрами, примерами применения и сведениями о семантическом версионировании.
6. В playbook добавлены обе авторские роли с версией `1.0.0`.
7. Монолитная конфигурация преобразована в три play: ClickHouse, Vector и LightHouse.
8. Для LightHouse продемонстрировано совместное использование `pre_tasks`, `roles` и `post_tasks`.
9. Дополнительными задачами создаются база и таблица ClickHouse, а также проверяется HTTP-доступность LightHouse.

## Ссылки на репозитории

- Роль Vector: <https://github.com/maksimdemin18/vector-role.git>
- Роль LightHouse: <https://github.com/maksimdemin18/lighthouse-role.git>
- Playbook: <https://github.com/maksimdemin18/08-ansible-04-role-playbook>

