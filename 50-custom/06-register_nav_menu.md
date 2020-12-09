# Меню
Создание меню.

## register_nav_menu (functions.php)
Регистрируем меню в functions.php:
https://wp-kama.ru/function/register_nav_menu

    add_action('after_setup_theme', 'test_after_setup');

    function test_after_setup(){
        register_nav_menu('top', 'Для шапки');
        register_nav_menu('footer', 'Для подвала');
    }

https://wp-kama.ru/function/register_nav_menus

## wp_nav_menu (head.php)
Внедряем меню в head.php:
https://wp-kama.ru/function/wp_nav_menu:

    wp_nav_menu([
        'theme_location' => 'top',
        'container' => null,
        'items_wrap' => '<ul>%3$s</ul>'
    ]);
---
## Создём меню
В файле functions.php прописываем:

    register_nav_menu('menu-top', 'Верхнее меню');

Заходим в админку, там в разделе "Внешний вид", должна появиться ссылка "Меню".

Чтобы вывести меню на сайте, нужно в теме прописать следующий код:

    wp_nav_menu(array(
        'theme_location' => 'menu-1',
        'container' => 'nav',
        'container_class' => 'navbar-collapse collapse',
        'menu_class' => 'nav navbar-nav navbar-right',
    ));
