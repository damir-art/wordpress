# Сайдбар
Создание сайдбара.

В файле functions.php прописываем:

    register_sidebar(
        array(
            'name' => 'Сайдбар',
            'id' => 'mainsidebar',
            'before_widget' => '<div class="widget">',
            'after_widget' => '</div>',
            'before_title' => '<div class="widgetTitle">',
            'after_title' => '</div>',
        )
    );

    /* у id не должно быть заглавных букв и пробелов */

В файле sidebar.php, прописываем:

    <?php if (!dynamic_sidebar('mainsidebar')): ?>
    <?php endif; ?>

    или

    if (function_exists('dynamic_sidebar')) {
        dynamic_sidebar('sidebar-$i');
    }

https://wp-kama.ru/function/register_sidebar

Регистрация в functions.php:

    add_action('widgets_init', 'test_widgets');

    function test_widgets(){
        register_sidebar([
            'name'          => 'Sidebar Top',
            'id'            => 'sidebar-top',
            'description'   => 'Шапка сайта',
            'before_widget' => '<div class="widget %2$s">',
            'after_widget'  => "</div>\n"
        ]);

        register_sidebar([
            'name'          => 'Sidebar Bottom',
            'id'            => 'sidebar-bottom',
            'description'   => 'Подвал сайта',
            'before_widget' => '<div class="widget %2$s">',
            'after_widget'  => "</div>\n"
        ]);
    }

## Разное
- register_sidebar() - регистрирует сайдбар в function.php
- register_sidebars() - регистрирует несколько сайдбаров сразу в function.php
- dynamic_sidebar() - выводит панель виджетов в шаблоне
- id - не должен быть равен https://wordpress.stackexchange.com/questions/59973/what-is-allowed-as-an-id-argument-in-register-sidebar-args/59985#59985

По-умолчанию, сайдбар с виджетами это список ul с li, каждый виджет имеет заголовк h2, 'name' и 'id' - должны быть заполнены, $i - инкремент от нуля, считающий количество сайдбаров в теме.


- `name` - название сайдбара, которое видно в админке, например 'Левый сайдбар'
- `Sidebar %d` - покажет "Боковая колонка 0"
- `id` - идентификатор сайдбара, нельзя использовать пробелы и заглавные буквы
- `description` - описание сайдбара, которое видно в админке, например 'Сайдбар который бла-бла-бла...'
- `class` - имя класса главного тега сайдбара
- `before_widget` - HTML-код, который будет расположен перед каждым виджетом
- `before_title` - HTML-код, который будет расположен
    перед каждым заголовком виджета

## Виджеты
Вывод в шаблоне любого произвольного виджета:

    the_widget('WP_Widget_Categories', 'param1', 'param2');
