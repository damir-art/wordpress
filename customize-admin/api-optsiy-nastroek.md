# API опций настроек
https://wp-kama.ru/id_3773/api-optsiy-nastroek.html

**API опций настроек** позволяют создавать поля форм (опции), в существующие страницы админки или страницы админки созданные пользователем (например для страниц плагина).

Для хранения опций в WordPress имеется отдельная таблица.

## Функции API
Регистрация/удаление опций
- register_setting()
- unregister_setting()

Добавление блоков и отдельных полей
- add_settings_section()
- add_settings_field()

Вывод на экран
- settings_fields()
- do_settings_sections()
- do_settings_fields()

Ошибки
- add_settings_error()
- get_settings_errors()
- settings_errors()

`register_setting()` и остальные функции опций вызываются в момент срабатывания хука `admin_init`.

Функцию реистрации опции можно поместить в функцию `legioner_add_admin_page()`.

Все три функциирегистрации опции, секции и поля взаимосвязаны:
