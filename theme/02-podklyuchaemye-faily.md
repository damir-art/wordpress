# Подключаемые файлы
После создания файл-шаблона style.css, нужно разделить index.html (главную страницу сверстанного макета сайта) на 3 или 4 (если есть сайдбар) части и разместить их в следующие файлы: index.php, header.php, footer.php (sidebar.php если есть).

Где header.php, footer.php, sidebar.php это и есть подключаемые файлы.

    header.php  - хранит код шапки сайта
    footer.php  - хранит код подвала сайта
    sidebar.php - хранит код сайдбара (боковой панели)

Размещаем в файл-шаблонах `index.php`, `single.php`, `page.php` и т.п:

    <?php get_header(); ?>  - шапка сайта
    <?php get_footer(); ?>  - подвал сайта
    <?php get_sidebar(); ?> - сайдбар сайта (если есть)

Размещаем в подключаемых файлах `header.php`, `footer.php`:

    <?php wp_head(); ?>   - перед </head>
    <?php wp_footer(); ?> - перед </body>

Для отдельных файл-шаблонов можно подключать одноименные подключаемые файлы:

    get_header($name)  - header-{name}.php
    get_header('home') - header-home.php
    get_sidebar($name) - sidebar-{name}.php
    get_footer($name)  - footer-{name}.php

get_header($name, $arr) - вторым параметром можно передавать массив

## wp_head(), wp_footer()
wp_head(), wp_footer() - крайне важные функции WordPress, отвечающие за совместимость плагинов, скриптов, подключения админ-панели и многое другое.

**wp_head()** - функция WordPress, которая подключает CSS-файлы, метатеги, скрипты плагинов, ядра WordPress и многое другое. Вставляется перед закрывающим тегом `head`.

**wp_footer()** - функция WordPress, которая отвечает за вывод панели администратора вверху сайта при входе в админку WordPress. Также данная функция, тоже подключает CSS и JS-файлы и многое другое. Вставляется перед закрывающим тегом `body`.

## Код header.php

    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Тема WordPress</title>
      <link rel="stylesheet" href="<?php bloginfo('template_url'); ?>/style.css" />
      <?php wp_head(); ?>
    </head>
    <body>
      <div class="container">
        <header>
          <a href="#" class="title-site">Название сайта</a>
        </header>
        <main>

## Код footer.php

        </main>
        <footer>
          <div>Подвал сайта</div>
        </footer>
      </div> <!-- .container -->
      <?php wp_footer(); ?>
    </body>
    </html>

## Код index.php

    <?php get_header(); ?>
    <!-- Код цикла -->
    <p>Цикл</p>
    <?php get_footer(); ?>

## Разное
- https://wp-kama.ru/function/get_header
- https://wp-kama.ru/function-cat/podklyuchenie-faylov - список всех подключаемых файлов
