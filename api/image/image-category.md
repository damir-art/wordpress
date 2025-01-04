# Изображения у категорий
В WordPress для вывода изображения категории можно использовать функцию `get_term_meta()`, чтобы получить URL изображения, привязанного к категории. Вот пример кода, который можно использовать в вашем шаблоне:

```php
$thumbnail_id = get_term_meta( $category_id, 'thumbnail_id', true );
$image = wp_get_attachment_url( $thumbnail_id );
if ( $image ) {
  echo '<img src="' . esc_url( $image ) . '" alt="' . esc_attr( $category->name ) . '">';
}
```

В этом примере `$category_id` - это ID категории, для которой вы хотите получить изображение. Функция `get_term_meta()` возвращает ID миниатюры, привязанной к категории, затем `wp_get_attachment_url()` используется для получения URL изображения по этому ID. Наконец, если изображение найдено, оно выводится с использованием HTML-тега `<img>`.

## Привязка изображений к категориям
Привязка изображения к категории в WordPress осуществляется через механизм метаполей (meta fields) для таксономий. Стандартный интерфейс администрирования WordPress не поддерживает прямое назначение изображений для категорий, однако это можно сделать с помощью дополнительных плагинов или вручную, используя код.

### Использование плагина
Самый простой способ — установить плагин, который добавляет поддержку изображений для категорий. Один из популярных плагинов — **Term Meta UI**. Он позволяет добавлять произвольные поля к таксономиям, включая категории.

1. Установите и активируйте плагин Term Meta UI.
2. Перейдите в админке WordPress в раздел **Категории** (в левом меню).
3. Выберите нужную категорию и нажмите кнопку **Редактировать**.
4. На странице редактирования категории появится новый блок **Meta Fields**, где вы сможете загрузить изображение и сохранить изменения.

### Ручная настройка через код
Если вы предпочитаете ручное решение, вот пошаговая инструкция:

Добавьте поле для загрузки изображения:  
Для этого добавьте следующий код в файл `functions.php` вашей активной темы или в отдельный плагин:

```php
// Добавляем новое поле для загрузки изображения
add_action( 'category_add_form_fields', 'add_image_upload_field' );
add_action( 'category_edit_form_fields', 'add_image_upload_field' );

function add_image_upload_field( $taxonomy ) {
?>
<div class="form-field term-group">
  <label for="category-image-id"><?php _e( 'Категория Изображение', 'your-text-domain' ); ?></label>
  <input type="hidden" id="category-image-id" name="category-image-id" value="" />
  <button type="button" class="upload-image-button button"><?php _e( 'Загрузить изображение', 'your-text-domain' ); ?></button>
  <span class="description"><?php _e( 'Нажмите кнопку, чтобы выбрать изображение.', 'your-text-domain' ); ?></span>
</div>
<?php
}
```

Добавьте JavaScript для загрузки изображения:  
Чтобы кнопка загрузки работала, добавьте следующий JavaScript-код в ваш файл `functions.php`:

```php
// Регистрация и подключение скрипта
add_action( 'admin_enqueue_scripts', 'load_media_uploader' );

function load_media_uploader() {
  wp_enqueue_media();
  wp_enqueue_script( 'media-uploader', get_template_directory_uri() . '/js/media-uploader.js', array( 'jquery' ), null, true );
}
```

Создайте файл `media-uploader.js` в папке `/js` вашей темы и добавьте туда следующий код:

```javascript
jQuery(document).ready(function($){
  var mediaUploader;

  $(document).on('click', '.upload-image-button', function(e) {
    e.preventDefault();

    if (mediaUploader) {
      mediaUploader.open();
      return;
    }

    mediaUploader = wp.media.frames.file_frame = wp.media({
      title: 'Выберите изображение для категории',
      button: {
        text: 'Использовать это изображение'
      },
      multiple: false
    });

    mediaUploader.on('select', function() {
      attachment = mediaUploader.state().get('selection').first().toJSON();
      $('#category-image-id').val(attachment.id);
      $('.upload-image-button').text('Изменить изображение');
    });

    mediaUploader.open();
  });
});
```

Сохраняйте изображение при сохранении категории:  
Чтобы сохранить ID изображения при сохранении категории, добавьте следующий код в `functions.php`:

```php
// Сохранение изображения при сохранении категории
add_action( 'edited_category', 'save_category_image', 10, 2 );
add_action( 'create_category', 'save_category_image', 10, 2 );

function save_category_image( $term_id, $tt_id ) {
  if ( isset( $_POST['category-image-id'] ) && '' !== $_POST['category-image-id'] ) {
    update_term_meta( $term_id, 'category-image-id', absint( $_POST['category-image-id'] ) );
  }
}
```

Вывод изображения категории:  
Теперь, когда изображение привязано к категории, вы можете вывести его следующим образом:

```php
$category_id = get_queried_object()->term_id; // Получаем ID текущей категории
$thumbnail_id = get_term_meta( $category_id, 'category-image-id', true );
$image = wp_get_attachment_url( $thumbnail_id );
if ( $image ) {
  echo '<img src="' . esc_url( $image ) . '" alt="' . esc_attr( $category->name ) . '">';
}
```

Этот код можно поместить в нужный шаблон (например, `category.php`), чтобы вывести изображение категории на странице архива категории.

## Заключение
Привязка изображения к категории в WordPress требует немного больше усилий, чем стандартные операции с записями, поскольку WordPress изначально не поддерживает этот функционал. Тем не менее, с помощью плагинов или ручной настройки вы сможете добиться желаемого результата.
