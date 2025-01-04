# Создание темы WordPress

Из каких файлов состоит шаблон:
- 404.php
- archive.php
- footer.php
- front-page.php
- functions.php
- header.php
- index.php
- page.php
- screenshot.png
- single.php
- style.css

<?php bloginfo('template_url'); ?>/

## Начальный functions.php
- когда пишите свои функции, например в файле `functions.php` вставляйте перед её именем имя темы, например `function theme_foo() {}`, это необходимо во избежании конфликта имён функций
- при разработке WordPress в инспекторе браузере во влкадке `Сеть`, отключайте кэш

Первая функция в function.php, должна быть функция `themeName_debug()` форматированно выводящая объекты и массивы:

    /**
    * Показать массив, объект
    */
    function themeName_debug( $elem ) {
      echo '<pre>';
      print_r( $elem );
      echo '</pre>';
    }

или:

    function legioner_debug( $data ) {
      echo '<pre>' . print_r( $data, 1 ) . '</pre>';
    }

Отмена сжатия изображений при загрузке:

    /**
    * Отменяем сжатие изображений
    */
    add_filter( 'jpeg_quality', function( $quality ) {
      return 100;
    });

Начальные настройки темы:

    /**
    * Начальные настройки темы
    */
    add_action( 'after_setup_theme', function() {
      // Создание метатега <title> через хук
      // add_theme_support( 'title-tag' ); // появится когда подключите функции wp_head() и wp_footer()

      // Поддержка миниатюр
      // add_theme_support('post-thumbnails');

      // Включаем меню в админке
      // add_theme_support( 'menus' );

      add_theme_support('html5', array(
        'comment-list',
        'comment-form',
        'search-form',
        'gallery',
        'caption',
        'script',
        'style',
      ));
    });

## Из чего состоит WordPress
WordPress это набор файлов, которые управляют контентом хранящимся в базе данных.

Файлы WordPress делятся на 4 вида:
1. Файлы ядра (движок): `/wp-admin/`
2. Файлы тем: `/wp-content/themes/`
3. Файлы плагинов: `/wp-content/plugins/`
4. Загруженные пользователем файлы (изображения, документы и т.п.): `/wp-content/uploads/`

## Создаём свою тему WordPress
**Тема** - это набор различных файлов и папок размещенных в определённом месте WordPress. У файлов должны быть определенные имена, относящиеся к WordPress. Внутри этих файлов размещается определённый код с использованием функций WordPress.  
**Файл-шаблон** - это файл, являющийся частью темы WordPress.  

Где хранятся темы WordPress? Темы хранятся по пути `/wp-content/themes/`

Чтобы создать свою тему и подключить её к WordPress, нужно в папке `/wp-content/themes/`, создать папку с каким нибудь именем, например `myTheme` и в этой папке разместить два файла: `style.css` и `index.php`

Прежде чем приступить к созданию своей темы, нужно:
- Скачать и установить последнюю версию CMS WordPress
- Создать 10 любых записей
- Создать по адресу /wp-content/themes/ папку myTheme
- В папке myTheme создать два пустых файла index.php и style.css

## index.php
В файл `index.php` добавляем код ниже. Это простой шаблон с шапкой, местом под цикл и подвалом. Файл стилей подключен.

    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Тема WordPress</title>
      <link rel="stylesheet" href="<?php bloginfo('template_url'); ?>/style.css" />
    </head>
    <body>
      <div class="container">
        <header>
          <a href="#" class="title-site">Название сайта</a>
        </header>
        <main>
          <p>Цикл</p>
        </main>
        <footer>
          <div>Подвал сайта</div>
        </footer>
      </div> <!-- .container -->
    </body>
    </html>

## style.css
Стили созданы с использованием grid. Подвал прижат к низу страницы.

    html,
    body {
      width: 100%;
      height: 100%;
    }

    body {
      margin: 0;
      color: #ffffff;
      font-family: 'Courier New', Courier, monospace;
    }

    .container {
      min-height: 100%;
      max-width: 1200px;
      margin: 0 auto;
      display: grid;
      gap: 16px;
      grid-template-rows: minmax(100px, auto) 1fr minmax(150px, auto);
      /* grid-template-columns: 100%; */
    }

    header {
      display: flex;
      align-items: center;
      background-color: #34495e;
      padding-left: 16px;
      padding-right: 16px;
    }

    .title-site {
      color: #ffffff;
      font-size: 24px;
      font-weight: bold;
      text-decoration: none;
    }

    main {
      background-color: #34495e;
      padding-left: 16px;
      padding-right: 16px;
    }

    footer {
      background-color: #34495e;
      padding-left: 16px;
      padding-right: 16px;
    }

## Структура верстки
Стандартная структура верстки темы WordPress:

    assets/
      css/
        style.css
      img/
        изображения дизайна сайта
      js/
        custom.js
    exp/
      bootstrap/
      другие расширения
    style.css
    index.php

## Разное
- название темы, это его ID и оно должно быть уникальным, отличаться от названий в репозитории

Получаем информацию о теме:

    // Получаем информацию о теме
    echo '<pre>';
    print_r(wp_get_theme());
    echo '</pre>';
