# custom-background
https://developer.wordpress.org/themes/functionality/custom-backgrounds/

Добавляет возможность настраивать цвет фона сайта или фонового изображения.

Подключаем возможность использования:

    add_theme_support(
      'custom-background',
      array (
        'default-color'          => '',
        'default-image'          => '',
        'wp-head-callback'       => '_custom_background_cb',
        'admin-head-callback'    => '',
        'admin-preview-callback' => ''
      )
    );

Массив параметров можно не указывать.  
В файл-шаблоны никаких функций внедрять не надо, всё настраивается сразу, благодаря `wp_head()` и `body_class()`.  
Фон изображения имеет приоритет над фоном цвета.

Пример со значениями:

    $another_args = array(
      'default-color'      => '0000ff',
      'default-image'      => get_template_directory_uri() . '/images/pic.jpg',
      'default-position-x' => 'right',
      'default-position-y' => 'top',
      'default-repeat'     => 'no-repeat',
    );
    add_theme_support( 'custom-background', $another_args );
