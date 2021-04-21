# Подключаемые файлы
После создания файл-шаблона style.css, нужно разделить index.html (главную страницу сверстанного макета сайта) на 3 или 4 (если есть сайдбар) части и разместить их в следующие файлы: index.php, header.php, footer.php, sidebar.php.

Где header.php, footer.php, sidebar.php это и есть подключаемые файлы.

    header.php  - хранит код шапки сайта
    footer.php  - хранит код подвала сайта
    sidebar.php - хранит код сайдбара (боковой панели)

Размещаем в файл-шаблонах:

    <?php get_header(); ?>  - шапка сайта
    <?php get_footer(); ?>  - подвал сайта
    <?php get_sidebar(); ?> - сайдбар сайта

Размещаем в подключаемых файлах:

    <?php wp_head(); ?>   - перед </head>
    <?php wp_footer(); ?> - перед </body>

Для отдельных файл-шаблонов можно подключать одноименные подключаемые файлы:

    get_header($name)  - header-{name}.php
    get_header('home') - header-home.php
    get_sidebar($name) - sidebar-{name}.php
    get_footer($name)  - footer-{name}.php

## wp_head(), wp_footer()
wp_head(), wp_footer() - крайне важные функции WordPress, отвечающие за совместимость плагинов, скриптов и многое другое.

**wp_head()** - функция WordPress, которая подключает CSS-файлы, метатеги, скрипты плагинов, ядра WordPress и многое другое. Вставляется перед закрывающим тегом `head`.

**wp_footer()** - функция WordPress, которая отвечает за вывод панели администратора вверху сайта при входе в админку WordPress. Также данная функция, тоже подключает CSS и JS-файлы и многое другое. Вставляется перед закрывающим тегом `body`.

## Разное
- https://wp-kama.ru/function/get_header
- https://wp-kama.ru/function-cat/podklyuchenie-faylov - список всех подключаемых файлов
