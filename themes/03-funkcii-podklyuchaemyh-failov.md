# Функции подключаемых файлов
После создания подключаемых файлов header.php, footer.php, туда нужно внедрить различные функции, рассмотрим их.

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
    <?php wp_body_open(); ?>

    <section class="section section--header">
      <h1><?php bloginfo('name'); ?></h1>
      <p><?php bloginfo('description'); ?></p>
    </section>

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

## wp_body_open()
Запускает хук `wp_body_open`. `<?php wp_body_open(); ?>` помещают сразу после открывающего тега `body`. С версии WP 5.2 эту функцию нужно использовать в теме (шаблоне), чтобы дать возможность разработчикам вставлять что-либо в самом начале тега `body`.

## Основные функции
Частоиспользуемые функции:

- `<?php get_header(); ?>` - header.php
- `<?php get_footer(); ?>` - footer.php
- `<?php get_sidebar(); ?>` - sidebar.php
- `get_stylesheet_uri()` - url файла темы style.css
- `get_stylesheet_directory()` - путь до темы (где доч./род. style.css), нет `/` в конце
- `get_template_directory()` - путь до род. темы (не дочерней), нет `/` в конце
- `get_template_directory_uri()` - url род. темы (не дочерней), нет `/` в конце
- `get_stylesheet_directory_uri()` - url темы (учитывает доч. темы), нет `/` в конце
- `<?php get_template_part('file'); ?>`-  file.php
- `<?php get_search_form(); ?>` - выводит форму поиска searchform.php в шаблоне
- `<?php comments_template(); ?>` - шаблон комментариев comments.php
- `get_parent_theme_file_path()` - путь файла род. темы (не дочерней)
- `get_theme_file_path()`- путь файла доч./род. темы
- `get_theme_root_uri()` - url папки с темами, нет `/` в конце
- `get_theme_file_uri()` - url файла доч./род. темы
- `get_parent_theme_file_uri()` - url файла род. темы
- `locate_template()` - находит/подключает файл доч./род. темы
- `load_template()` -  подключает файл (require_once)

## Весь код

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
    <?php wp_body_open(); ?>

    <section class="section section--header">
      <h1><?php bloginfo('name'); ?></h1>
      <p><?php bloginfo('description'); ?></p>
    </section>

    <section class="section section--footer">
      footer
    </section>

    <?php wp_footer(); ?>
    </body>
    </html>