# Пользовательские типы записей
## register_post_type()

Создание пользовательских типов записей.

**Важно:** после создания нового типа записи, обязательно нужно зайти в админке в `Настройки > Постоянные ссылки`. Это необходимо для того, чтобы правила ЧПУ работали и для нового типа записи.

* https://wp-kama.ru/id_5851/zapisi-v-wordpress.html
* https://wp-kama.ru/function/register_post_type
* **Плагин:** https://ru.wordpress.org/plugins/custom-post-type-ui/

По-умолчанию в WordPress существует два типа записей (типа контента) это посты и страницы.

Пример:

    add_action( 'init', 'foo' );
    function foo() {
        register_post_type( 'news', [
            'key' => 'value',
            ...
        ]);
    }