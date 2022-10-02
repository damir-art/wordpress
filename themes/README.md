# Создание темы WordPress
Содержание раздела:
- style.css
- подключаемые файлы (header.php, footer.php, sidebar.php)
- функции подключаемых файлов
- index.php
- иерархия шаблонов
- цикл (loop)
- single.php
- page.php
- 404.php
- search.php
- category.php

## Из чего состоит WordPress
WordPress это набор файлов, которые управляют контентом хранящимся в базе данных.

Файлы WordPress делятся на 4 вида:
1. Файлы ядра (движок): `/wp-admin/`
2. Файлы тем: `/wp-content/themes/`
3. Файлы плагинов: `/wp-content/plugins/`
4. Загруженные пользователем файлы (изображения, документы и т.п.): `/wp-content/uploads/`

## Где создаём тему
Тему создаём в папке `/wp-content/themes/`:
- При именовании темы используйте префикс (этот же префикс вы будете использовать при именовании своих функций)
- Создаём папку например `dmsite`, где `dm` это префикс
- Внутрь папки `dmsite` помещаем два файла: style.css и index.php
- Тема готова

Далее работаем с файлами style.css и index.php

## index.php
Простой шаблон с шапкой, циклом и подвалом. Файл стилей подключен.

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

    /*
        Theme Name: Название темы dmSite
        Description: Краткое описание темы dmSIte
        Author: Имя автора темы dmSite
    */

    /* @import url('css/style.css'); */

    html, body {
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
      grid-template-columns: 100%;
    }

    header, main, footer {
      background-color: #34495e;
      padding-left: 16px;
      padding-right: 16px;
    }

  header {
    display: flex;
    align-items: center;
  }

  .title-site {
    color: #ffffff;
    font-size: 24px;
    font-weight: bold;
    text-decoration: none;
  }

## Файл-шаблоны темы

    css/
    design/
    js/
    404.php
    footer.php
    functions.php
    header.php
    index.php
    page.php
    screenshot.png (1200x900px)
    single.php
    style.css

Файлы в основном относятся к теме WordPress и имеют определённые названия, папки темы можно называть как угодно.

## Создаём свою тему WordPress
**Тема** - это набор файлов различных и файлов.

**Файл-шаблон** - это файл, являющийся частью темы WordPress.

Где хранятся темы WordPress? Темы хранятся по пути `/wp-content/themes/`

Чтобы создать свою тему и подключить её к WordPress, нужно в папке `themes`, создать папку с каким нибудь именем, например `myTheme` и в этой папке разместить два файла: style.css и index.php

Прежде чем приступить к созданию своей темы, нужно:
- Скачать и установить последнюю версию CMS WordPress
- Создать 10 любых записей
- Создать по адресу /wp-content/themes/ папку myTheme
- В папке myTheme создать два пустых файла index.php и style.css

## Создаём дочернюю тему
- в папке `/wp-content/themes/` создаём тему с именем `themeName-child`
- в папке `/wp-content/themes/themeName-child` создаём файл `style.css`

В файле `style.css` записываем:

    /*
        Theme Name: themeName-child
        Template: themeName
    */

    @import url('../themeName/style.css');

- дочерняя тема перед за основу файл-шаблоны родительской
  - копируем файл-шаблон родительской, подстраиваем под себя в дочерней
- function.php не перезаписывается, а дополняется
  - сначала срабатывается function.php из дочерней, потом из родительской
