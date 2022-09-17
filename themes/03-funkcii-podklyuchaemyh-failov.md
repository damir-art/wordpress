# Функции подключаемых файлов
После создания подключаемых файлов, туда нужно внедрить различные функции, рассмотрим их.

## header.php
Схема header.php:

    <!DOCTYPE html>
    <html <?php language_attributes(); ?>>
        <head>
            <meta charset="<?php bloginfo('charset'); ?>" />
            <title><?php wp_title ('|', true, 'right'); bloginfo('name'); ?></title>
            <link rel="stylesheet" href="<?php bloginfo('stylesheet_url'); ?>" />
            <link rel="profile" href="http://gmpg.org/xfn/11" />
            <link rel="pingback" href="<?php bloginfo('pingback_url'); ?>" />
            <link rel="alternate" type="application/rss+xml" title="<?php bloginfo('name'); ?> &raquo; Лента" href="<?php bloginfo('rss2_url'); ?>" />
            <?php wp_head(); ?> 
        </head>
        <body <?php body_class() ?> >
            <div id="header">
                <h1><?php bloginfo('name'); ?></h1>
                <p><?php bloginfo('description'); ?></p>
            </div>

Где:

- `language_attributes();` - устанавливает язык в зависимости от настроек WP
- `bloginfo('charset');` - определение кодировки сайта
- `bloginfo('name');` - вывод имени блога
- `bloginfo('stylesheet_url');` - подключение style.css темы или `<?php echo get_stylesheet_uri(); ?>`
- `bloginfo('description');` - вывод описания блога
- `echo get_option('home');` - вывод адреса блога
- `bloginfo('template_url');` - путь к теме
- `echo get_option('home');` - вывод адреса главной страницы сайта
- `wp_enqueue_script("jquery");` - подключаем jQuery, данную строку располагаем перед `wp_head();`
- `echo home_url();` - используем вместо bloginfo() с параметрами url, home, siteurl, wpurl
- `echo get_template_directory_uri();` - выводит путь к теме
- `body_class();` - добавление классов WordPress в тег body

Добавляем свой CSS-класс для внутренних страниц сайта:

    $body_class = '';
    if ( !is_front_page() ) {
        $body_class = 'inner';
    }

    <body class=" <?php echo $body_class ?> ">

Выяснить, что такое: `http://gmpg.org/xfn/11` и `bloginfo('pingback_url');`

`wp_head()` - вызывает `do_action( 'wp_head' `, на этот хук, WordPress подписывает вывод тегов которые нужно вставить между тегами `head`

## footer.php
Перед закрывающим тегом `body` удаляем все скрипты и вставляем `<?php wp_footer() ?>`

`wp_footer()` - вызывает хук `wp_footer`, WordPress подключает сюда скрипты, которые нужно вставить в футер. Также этот хук выводит панель администратора сверху.
