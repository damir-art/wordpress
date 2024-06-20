# Сайдбар
https://wp-kama.ru/function/register_sidebar  
https://wp-kama.ru/function/register_sidebars  

Сайдбар это зона, где с помощью виджетов выводится контент: `Админка > Внешний вид > Виджеты`. Также имеется одноимённый шаблон sidebar.php

- регистрация сайдбара `register_sidebar()`
- наполнение сайдбара виджетами (заголовок для виджета задается отдельно, как отдельный блок, эти блоки потом можно сгруппировать чтобы например у заголовка и виджета была единая HTML-обертка)
- вывод сайдбара `dynamic_sidebar()`, в шаблоне `sidebar.php` или любом другом: header.php, footer.php и т.п.

Рабочий процесс:
- register_sidebar() - регистрирует сайдбар в function.php
- register_sidebars() - регистрирует несколько сайдбаров сразу в function.php
- dynamic_sidebar() - выводит панель виджетов в шаблоне
- id - не должен быть равен https://wordpress.stackexchange.com/questions/59973/what-is-allowed-as-an-id-argument-in-register-sidebar-args/59985#59985

## Регистрация сайдара в function.php
Код регистрации сайдбара с виджетами в functions.php:

    /**
    * Регистрация сайдбара с виджетами
    */
    function legioner_widgets_init(){
      register_sidebar(
        array(
          'name' => 'Сайдбар в шапке сайта',
          'id' => 'sidebar-header',
          'before_widget' => '<div id="%1$s" class="widget %2$s">',
          'after_widget' => '</div>'
        )
      );
    }
    add_action( 'widgets_init', 'legioner_widgets_init' );

## Вывод сайдбара в шаблоне

    if ( function_exists('dynamic_sidebar') )
      dynamic_sidebar('sidebar-header');

## Старая запись

В файле `functions.php` прописываем:

    add_action( 'widgets_init', 'po_sidebar' );

    function po_sidebar() {
        register_sidebar([
            'name'          => 'Сайдбар для шапки',       // Название которое увидим в админке
            'id'            => 'sidebar-top',             // Используем в шаблоне при выводе
            'description'   => 'Изменяем номер телефона', // Описание, появится в админке
            'class'         => 'po-class-sidebar-top',    // CSS-класс панели виджетов
            'before_widget' => null,                      // null - убираем обертку виджета, можем поменять тег на div
            'after_widget'  => null                       // null - убираем конечный тег обертки виджета
        ]);

        register_sidebar([
            'name'          => 'Сайдбар для подвала',
            'id'            => 'sidebar-bottom',
            'description'   => 'Изменяем контакты',
            'class'         => 'po-class-sidebar-bottom',
            'before_widget' => null,
            'after_widget'  => null
        ]);
    }

У `id` не должно быть заглавных букв и пробелов.

В файл-шаблоне прописываем:

    <?php dynamic_sidebar( 'sidebar-top' ); ?>

И куча других способов вывода с проверками: https://wp-kama.ru/function/dynamic_sidebar

    // Выводим, если в сайдбаре есть хотя бы один виджет
    <?php if ( is_active_sidebar( 'sidebar-top' ) ) : ?>
        <ul id="sidebar">
            <?php dynamic_sidebar( 'sidebar-top' ); ?>
        </ul>
    <?php endif; ?>

## Шаблон register_sidebar

    add_action('widgets_init', 'test_widgets');

    function test_widgets(){
        register_sidebar([
            'name'           => 'Сайдбар для шапки',                  // Название которое увидим в админке
            'id'             => 'sidebar-top',                        // Используем в шаблоне при выводе
            'description'    => 'Изменяем номер телефона',            // Описание, появится в админке
            'class'          => 'po_class_sidebar',                   // CSS-класс панели виджетов
            'before_widget'  => '<li id="%1$s" class="widget %2$s">', // null - убираем обертку виджета, можем поменять тег на div
            'after_widget'   => "</li>\n",                            // null - убираем конечный тег обертки виджета
            'before_title'   => '<h2 class="widgettitle">',           // null - убираем заголовок виджета, можем поменять тег на div
            'after_title'    => "</h2>\n",                            // null - убираем конечный тег заголовка виджета,
            'before_sidebar' => '', // WP 5.6
            'after_sidebar'  => '', // WP 5.6
        ]);

        register_sidebar([
            'name'          => 'Сайдбар для подвала',
            'id'            => 'sidebar-bottom',
            'description'   => 'Изменяем контакты',
            'before_widget' => '<div class="widget %2$s">',
            'after_widget'  => "</div>\n"
        ]);
    }

`name` и `id` обязательны для использования.

По-умолчанию, сайдбар с виджетами это список `ul` с `li`, каждый виджет имеет заголовк `h2`. `name` и `id` - должны быть заполнены, `$i` - инкремент от нуля, считающий количество сайдбаров в теме.

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
