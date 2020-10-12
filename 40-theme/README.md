# Создание темы WordPress

- style.css
- Подключаемые файлы (header.php, footer.php, sidebar.php)
- index.php (loop)
- single.php
- page.php
- 404.php
- Иерархия шаблонов
- Функции используемые в файл-шблонах
- functions.php

## Файл-шаблоны темы
Создаем в папке с темой `/wp-content/themes/mythem`, папки и файлы:

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
