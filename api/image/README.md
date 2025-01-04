# API image
Функции для работы с изображениями.

WordPress предоставляет множество функций для работы с изображениями, начиная от загрузки и обработки до отображения и управления миниатюрами. Вот 30 наиболее популярных функций с краткими описаниями и примерами их использования.

## the_post_thumbnail()
Выводит миниатюру текущего поста или страницы.

```php
<?php the_post_thumbnail( 'medium' ); ?>
```

## has_post_thumbnail()
Проверяет, существует ли миниатюра у текущего поста.

```php
<?php if ( has_post_thumbnail() ) : ?>
  <?php the_post_thumbnail('full'); ?>
<?php else : ?>
  <img src="<?php echo get_template_directory_uri(); ?>/images/default-thumbnail.jpg" alt="Default Thumbnail">
<?php endif; ?>
```

## set_post_thumbnail()
Устанавливает миниатюру для указанного поста.

```php
<?php set_post_thumbnail($post_id, $attachment_id); ?>
```

## get_post_thumbnail_id()
Возвращает ID миниатюры текущего поста.

```php
<?php $thumbnail_id = get_post_thumbnail_id(); ?>
```

## get_the_post_thumbnail()
Возвращает HTML-код миниатюры текущего поста.

```php
<?php echo get_the_post_thumbnail($post_id, 'large'); ?>
```

## get_attached_file()
Возвращает полный путь к файлу вложения.

```php
<?php $file_path = get_attached_file($attachment_id); ?>
```

## wp_get_attachment_image()
Возвращает HTML-код изображения-вложения.

```php
<?php echo wp_get_attachment_image($attachment_id, 'medium'); ?>
```

## wp_get_attachment_image_src()
Возвращает массив с URL изображения, шириной и высотой.

```php
<?php list($src, $width, $height) = wp_get_attachment_image_src($attachment_id, 'full'); ?>
```

## wp_get_attachment_metadata()
Возвращает метаданные изображения-вложения.

```php
<?php $metadata = wp_get_attachment_metadata($attachment_id); ?>
```

## wp_generate_attachment_metadata()
Генерирует метаданные для нового изображения-вложения.

```php
<?php $metadata = wp_generate_attachment_metadata($attachment_id, $file); ?>
```

## wp_delete_attachment()
Удаляет указанное вложение.

```php
<?php wp_delete_attachment($attachment_id, true); ?>
```

## media_handle_upload()
Загружает файл и создаёт новое вложение.

```php
<?php $attachment_id = media_handle_upload('my_file_input_name', $post_id); ?>
```

## wp_crop_image()
Обрезает изображение.

```php
<?php $cropped_image = wp_crop_image($file, $src_x, $src_y, $src_w, $src_h, $dst_w, $dst_h, $dst_w, $dst_h); ?>
```

## wp_get_original_image_path()
Возвращает путь к оригинальному изображению.

```php
<?php $original_image_path = wp_get_original_image_path($attachment_id); ?>
```

## wp_get_image_editor()
Создаёт объект редактора изображений.

```php
<?php $editor = wp_get_image_editor($file); ?>
```

## image_downsize()
Возвращает URL уменьшенной версии изображения.

```php
<?php list($url, $width, $height) = image_downsize($attachment_id, 'thumbnail'); ?>
```

## add_image_size()
Регистрирует новый размер изображения.

```php
<?php add_image_size('custom-size', 800, 600, true); ?>
```

## remove_image_size()
Удаляет зарегистрированный размер изображения.

```php
<?php remove_image_size('custom-size'); ?>
```

## get_intermediate_image_sizes()
Возвращает массив зарегистрированных размеров изображений.

```php
<?php $sizes = get_intermediate_image_sizes(); ?>
```

## wp_get_attachment_thumb_file()
Возвращает путь к миниатюре вложения.

```php
<?php $thumb_file = wp_get_attachment_thumb_file($attachment_id); ?>
```

## wp_get_attachment_thumb_url()
Возвращает URL миниатюры вложения.

```php
<?php $thumb_url = wp_get_attachment_thumb_url($attachment_id); ?>
```

## get_intermediate_image_sizes()
Возвращает массив промежуточных размеров изображений.

```php
<?php $intermediate_sizes = get_intermediate_image_sizes(); ?>
```

## update_attached_file()
Обновляет путь к файлу вложения.

```php
<?php update_attached_file($attachment_id, $new_file_path); ?>
```

## wp_update_attachment_metadata()
Обновляет метаданные изображения-вложения.

```php
<?php wp_update_attachment_metadata($attachment_id, $new_metadata); ?>
```

## get_media_item()
Возвращает данные об изображении-вложении.

```php
<?php $media_item = get_media_item($attachment_id); ?>
```

## wp_prepare_attachment_for_js()
Подготавливает данные вложения для использования в JavaScript.

```php
<?php $attachment_data = wp_prepare_attachment_for_js($attachment_id); ?>
```

## get_attached_media()
Возвращает массив вложений определенного типа для заданного поста.

```php
<?php $attached_images = get_attached_media('image', $post_id); ?>
```

## wp_attachment_is_image()
Проверяет, является ли вложение изображением.

```php
<?php if ( wp_attachment_is_image($attachment_id) ) { /* ... */ } ?>
```

## get_post_gallery()
Возвращает галерею изображений, прикрепленных к посту.

```php
<?php $gallery = get_post_gallery($post, false); ?>
```

## wp_enqueue_media()
Включает скрипты и стили медиабиблиотеки WordPress.

```php
<?php wp_enqueue_media(); ?>
```

---

Эти функции предоставляют мощные инструменты для работы с изображениями в WordPress, позволяя загружать, обрабатывать, управлять и отображать изображения различными способами.
