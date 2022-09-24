# functions.php
functions.php - файл темы, с помощью которого можно взаимодействовать с ядром WordPress. В этом файле можно использовать функции хуки и работать с ядром WordPress.  
functions.php - отрабатывает самым первым, функции внутри этого фйла должны возвращать данные.

Скрипты расположенные в файле functions.php, также еще называют **сниппетами**.

**functions.php** - это специальный файл темы WordPress, с помощью которого можно подключить к своему сайту, возможность использования виджетов, меню, миниатюр и много чего еще.

Файл functions.php является одним из важнейших файлов темы, благодаря которому с помощью написания небольших скриптов на PHP, можно делать со своим сайтом всё что угодно.

Файл functions.php, подключать к файл-шаблонам не нужно, он запускается автоматически при открытии сайта, главное разместите данный файл в папку с вашей темой, а всё остальное сделает за вас WordPress. Также при отсутствии в подключаемых файлах header.php и footer.php, функций wp_head() и wp_footer(), файл functions.php работать не будет.

Основное предназначение файла functions.php - это расширение или изменение базового функционала ядра WordPress. Файл functions.php может изменять основной функционал ядра WordPress или отдельного плагина, с помощью хуков.

functions.php - запускается сразу, до запуска темы WordPress. Например если что-нибудь внедрить в этот файл, то сначала выполнится скрипт в файле functions.php, а уже потом ядро WordPress и активная тема.

Если в файле functions.php ввести `echo 1`, то единичка появится в исходном коде, до DOCTYPE. Поэтому напрямую в файл functions.php ничего не пишут, а ипользуют **хуки**.

## Задачи functions.php
Основные задачи, которые выполняет файл functions.php:
- Грамотно подключает CSS и JavaScript файлы, которые участвуют в формировании темы;
- Создаёт возможность использования на сайте меню, виджетов, миниатюр и др;
- С помощью скриптов (сниппетов) расширяет или изменяет базовый функционал ядра WordPress и плагинов WordPress;
- Регистрирует различные шорткоды;
- Создаёт пользовательские типы записей;
- Создаёт пользовательские таксономии;
- Создаёт произвольные поля;
- и многое, многое другое

Различные скрипты файла functions.php

## show_admin_bar
Удаляем админскую панель из фронта (зачем?)
    
    add_filter('show_admin_bar', '__return_false');

## wp_enqueue_scripts
    add_action('wp_enqueue_scripts', 'test_media');

    function test_media(){
        wp_enqueue_style('test-main', get_stylesheet_uri());
        wp_enqueue_script('test-script-main', get_template_directory_uri() . '/assets/js/script.js');
    }

## after_setup_theme
    add_action('after_setup_theme', 'test_after_setup');

    function test_after_setup(){
        register_nav_menu('top', 'Для шапки');
        register_nav_menu('footer', 'Для подвала');
        
        add_theme_support('post-thumbnails');
        add_theme_support('title-tag');
    }

## widgets_init
    add_action('widgets_init', 'test_widgets');

    function test_widgets(){
        register_sidebar([
            'name' => 'Sidebar Right',
            'id' => 'sidebar-right',
            'description' => 'Правая колонка',
            'before_widget' => '&lt;div class="widget %2$s">',
            'after_widget'  => "&lt;/div>\n"
        ]);
        
        register_sidebar([
            'name' => 'Sidebar Top',
            'id' => 'sidebar-top',
            'description' => 'Странный пример',
            'before_widget' => '&lt;div class="widget %2$s">',
            'after_widget'  => "&lt;/div>\n"
        ]);
    }

## Поддержка настраиваемых изображений в шапке сайта
В functions.php

    $args = array(
        'flex-width'    => false,
        'width'         => 960,
        'flex-height'   => true,
        'height'        => 200,
    );
    add_theme_support( 'custom-header', $args );

В header.php

    <?php
        if (get_header_image()) {
    ?>
            <img src="<?php header_image(); ?>" height="<?php echo get_custom_header()->height; ?>" width="<?php echo get_custom_header()->width; ?>" alt="" />
    <?php
        }
    ?>