# Добавляем функционал в тему
Настраиваем нашу тему, в основном работаем через файл `functions.php`

* Подключаем стили и скрипты
* Подключаем элементы, которые должна поддерживать тема
* Пользовательские типы записей `register_post_type()`
* Пользовательские таксономии
* Пользовательские поля
* Пользовательские избражения

Запрещено использовать следующие имена: https://wp-kama.ru/id_7648/zanyatye-peremennyh-zaprosa.html

## Первый хук
Включаем поддержку добавления логотипа в тему. Внешний вид > Настроить > Свойства сайта > Логотип.

    add_action( 'after_setup_theme', 'po_setup' );

    function po_setup () {
        add_theme_support( 'custom-logo' );
        add_theme_support( 'title-tag' );      // поддержка title
        add_theme_support( 'post-thumbnails' ); // поддержка миниатюр
    }

Выводим логотип на страницу. В `header.php` записываем:

    <?php the_custom_logo() ?>
