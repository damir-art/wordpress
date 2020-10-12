# functions.php
functions.php - запускается сразу, до запуска темы WordPress. Например если что-нибудь внедрить в этот файл, то сначала выполнится скрипт в файле functions.php, а уже потом ядро WordPress и активная тема.

Если в файле functions.php ввести `echo 1`, то единичка появится в исходном коде, до объявления версии HTML. Поэтому напрямую в файл functions.php ничего не пишут, а ипользуют **хуки** *(крючки)* - событийную модель.

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
