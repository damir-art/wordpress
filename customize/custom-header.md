# custom-header
https://wp-kama.ru/function/add_theme_support#custom-header  
Вставляем изображение или видео в шапку сайта.

    add_theme_support( 
      'custom-header',
      array(
        'default-image'          => '',
        'random-default'         => false,
        'width'                  => 0,
        'height'                 => 0,
        'flex-height'            => false,
        'flex-width'             => false,
        'default-text-color'     => '', // цвет возвращается функцией get_header_textcolor()
        'header-text'            => true,
        'uploads'                => true,
        'wp-head-callback'       => '',
        'admin-head-callback'    => '',
        'admin-preview-callback' => '',
        'video'                  => false, // с 4.7
        'video-active-callback'  => 'is_front_page', // с 4.7
      )
    );

## Функции
Все функции для вывода картинки и видео заголовка:

- `get_header_image()` - получает УРЛ картинки шапки (заголовка), для вывода на экран есть обертка header_image()
- `get_header_image_tag()` - Получает img-тег картинки заголовка, добавляет туда атрибуты srcset и srcset, для вывода на экран есть обертка `the_header_image_tag()`
- `has_header_video()` - проверяет было ли установлено видео в кастомайзере
- `is_header_video_active()` - проверяет должно ли показываться видео для текущей страницы запроса во фронте
- `get_header_video_url()` - получает URL видео для шапки, может быть локальной или внешней ссылкой на видео
- `the_header_video_url()` - выводит URL видео для шапки
- `has_custom_header()` - проверяет установлена ли картинка для шапки темы, или установлено ли видео и доступно для показа на текущей странице запроса
- `get_custom_header_markup()` - получает HTML код для отображения картинки заголовка (не включает видео)
- `the_custom_header_markup()` - выводит HTML код для отображения картинки заголовка и подключает JS код для обработки видео заголовка, если оно доступно для текущего запроса
- `get_custom_header()` - возвращает данные изображения заголовка
- `register_default_headers()` - регистрация изображения заголовка по-умолчанию

## Пример
В functions.php добавляем:

    add_theme_support(
      'custom-header',
      array (
        'default-image'       => '',
        'height'              => 700,
        'width'               => 1900,
        'default-text-color'  => '#ffffff', // в настройках темы в разделе "Цвета" появится "Цвет текста заголовка"
      )
    );

В настройках темы появится раздел `Изображение заголовка`.

В header.php добавляем:

    <section style="background-image: url(<?php echo get_header_image() ?>);"