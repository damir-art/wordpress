# Пользовательские изображения
## add_image_size()
Создание пользовательских размеров изображений.

    // Поддержка изображений
    add_theme_support('post-thumbnails');

    // Создаем пользовательское изображение
    if(function_exists('add_image_size')) {
        add_image_size('name', 200, 400, true); // true - обрезка по центру, с масштабированием
    }