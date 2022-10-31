# Создаём логотип в шапке custom-logo
`Админка > Внешний вид > Настроить > Свойства сайта`  
Добавляем возможность загрузить изображение логотипа в настройках темы:

В functions.php помещаем:

    add_theme_support( 
      'custom-logo',
      array(
        'height'      => 190, // высота
        'width'       => 190, // ширина
        'flex-width'  => false, // обрезать при загрузки пропорционально
        'flex-height' => false, // обрезать при загрузки пропорционально
        'header-text' => '',
        'unlink-homepage-logo' => true, // удаляет ссылку на главной
      )
    );

`flex-*` - при true, изображение также не уменьшается до размеров height и width

В header.php помещаем:

    the_custom_logo()

- `the_custom_logo()` - выводим логотип
- `get_custom_logo()` - получаем логотип
- `has_custom_logo()` - проверяем логотип на существование

Получаем только URL картинки:

    $custom_logo__url = wp_get_attachment_image_src( get_theme_mod( 'custom_logo' ), 'full' ); 
    echo $custom_logo__url[0];

## Разное
- если обрезка ен появляется, то нажмите `ctrl -` для уменьшения мастаба страницы