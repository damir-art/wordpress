# Метабоксы
https://wp-kama.ru/function/add_meta_box  
https://wordpress.org/plugins/advanced-custom-fields/  

Произвольные метабоксы. В отличии от метабокса `Произвольные поля` они работают не только с текстовыми полями но и с другими, например чекбоксами, селектами, изображениями и т.п.

Метабокс - это при редактировании поста, например: статус и видимость, шаблон (если установлен для записи), постоянная ссылка, изображение записи (миниатюра) и т.д.

## Добавление add_meta_box()
Добавляет дополнительные блоки (meta box) на страницы редактирования/создания постов, постоянных страниц или произвольных типов записей в админ-панели.

Чтобы добавить свой метабокс, нужно использовать функцию `add_meta_box()`:

    add_meta_box( $id, $title, $callback, $screen )

- $id - идентификатор метабокса (обязателен)
- $title - заголовок метабокса (обязателен)
- $callback - функция которая выводит HTML-код метабокса (обязателен)
- $screen - указываем типы записей для которых доступен метабокс, строка или массив

## Код метабокса
Код даже одного метабокса получается довольно большой, поэтому их лучше размещать в отдельном файле, а потом подключать к файлу `functions.php`.

- назовём файл `legioner_metabox.php`
- поместим файл в папку `functions`
- подключим файл require `require __DIR__ . '/functions/legioner_metabox.php';`

Рабочий код файла `legioner_metabox.php`, который выводит сообщение на странице редактирования поста:

    /**
    * Создание метабокса
    */
    function legioner_metabox() {
      add_meta_box( `legioner_metabox_text`, 'Текстовое поле', 'legioner_metabox_cb', array( 'post', 'legioner_schedule') );
    }

    // Код HTML
    function legioner_metabox_cb( $post ) {
      // $post - хранит информацию о посте
      echo 'Hello World!';
    }

    // Вешаем на хук
    add_action( 'add_meta_boxes', 'legioner_metabox' );

## Полный код создания метабокса
- теги form создвать не нужно
- при записи инструкции `wp_nonce_field( 'legioner_action', 'legioner_nonce' );` (делать не обязательно), появятся скрытые поля input и числовое значение у одного поля

Код:

    <?php
    /**
    * Создание метабокса
    */
    function legioner_metabox() {
      add_meta_box( `legioner_metabox_text`, 'Заголовок метабокса', 'legioner_metabox_cb', array( 'post', 'legioner_schedule') );
    }

    // Код HTML
    function legioner_metabox_cb( $post ) {
      // $post - хранит информацию о посте
      legioner_debug(get_post_meta( $post->ID ));
      // Для безопасности wp_nonce_field(), можно не прописывать
      wp_nonce_field( 'legioner_action', 'legioner_nonce' );
      $legioner_text = get_post_meta( $post->ID, 'legioner-text', true ); // получаем текущее значение поля из БД (его нужно сохранить в value поля)
      $legioner_checkbox = get_post_meta( $post->ID, 'legioner-checkbox', true ); // получаем текущее значение поля из БД (его нужно вписать в тег чекбокса)
    ?>
      <!-- Используются CSS-классы WordPress -->
      <table class="form-table">
        <tbody>
          <!-- Элемент метабокса текстовое поле -->
          <tr>
            <th><label for="legioner-text">Заголовок текстового поля</label></th>
            <td><input class="regular-text" type="text" id="legioner-text" name="legioner-text" value="<?php echo esc_attr($legioner_text); ?>" /></td>
          </tr>
          <!-- Элемент метабокса чекбокс (по идее у одного метабокча должно быть одно поле) -->
          <tr>
            <th><label for="legioner-checkbox">Заголовок чекбокса</label></th>
            <td><input type="checkbox" id="legioner-checkbox" name="legioner-checkbox" <?php checked( 'yes', $legioner_checkbox ); ?>></td>
          </tr>
        </tbody>
      </table>
    <?php
    }
    add_action( 'add_meta_boxes', 'legioner_metabox' );

    // Сохраняем значения в БД
    function legioner_metabox_save( $post_id ) {
      // если поля не существует или оно существует но не проходит проверку
      if ( ! isset( $_POST['legioner_nonce'] ) || ! wp_verify_nonce( $_POST['legioner_nonce'], 'legioner_action' )) {
        return;
      }

      // если это автосохранение ничего не делаем
      if ( defined('DOING_AUTOSAVE') && DOING_AUTOSAVE ) {
        return;
      }

      // проверяем права юзера
      if( ! current_user_can( 'edit_post', $post_id ) ) {
        return;
      }

      // текстовое поле
      if( ! empty( $_POST['legioner-text'] ) ) {
        // если текстовое поле не пустое (что-то пришло от прльзователя), то обновляем данные
        update_post_meta( $post_id, 'legioner-text', sanitize_text_field( $_POST['legioner-text'] ) );
      } else {
        // иначе очищаем поле
        delete_post_meta( $post_id, 'legioner-text' );
      }

      // чекбокс поле
      if( isset( $_POST['legioner-checkbox'] ) && $_POST['legioner-checkbox'] == 'on' ) {
        // если чекбокс установлен в true
        update_post_meta( $post_id, 'legioner-checkbox', 'yes' );
      } else {
        // иначе очищаем поле
        delete_post_meta( $post_id, 'legioner-checkbox' );
      }
    }
    add_action( 'save_post', 'legioner_metabox_save' );

В произвольных полях, тоже появятся наши метабоксы.

## Вывод данных метабокса
Выводим значения пользовательских метабоксов:

    echo get_post_meta( get_the_ID(), 'legioner-text', true );

## Старая запись
- прописываем `add_post_meta()/update_post_meta()` в `function.php`, создание мета-поля
- `get_post_meta()` получение значения мета-поля

Шаблон:

    add_post_meta( $id,  $slug, $value )

- `$id`    - id записи, поста к которому добавляются мета-поля
- `$slug`  - ярлык, ключ мета-поля
- `$value` - значение

Регистрация мета-поля и добавления его в админку, пишем в `function.php`:

    // Регистрация мета-полей
    add_action( 'add_meta_boxes', 'po_meta_boxes' );
    function po_meta_boxes() {
        add_meta_box( 
            'po-news-id-html',
            'Название газеты',
            'po_meta_box_html',
            'news'
        );
    }

    function po_meta_box_html( $post_obj ) {
        $name_brand = get_post_meta( $post_obj -> ID, 'po-news', true );
        $name_brand = $name_brand ? $name_brand : 0;
        echo '<h1>' . $name_brand . '</h1>';
    }

- `add_meta_boxes` - хук срабатывает когда WordPress добавляет мета-поля для редактирования в админку
- `add_meta_box` - добавление поля
- `po-news-id-html` - id HTML-контейнера в котором будет находится мета-поле
- `Название газеты` - название мета-поля, виден в админке
- `po_meta_box_html` - функция которая отвечает за HTML-верстку мета-поля
- `news` - список типов записей, куда добавляется это поле, может быть массивом `[ 'post' , 'news' ]`
- `$context` - куда именно добавить поле в админке
- `$priority` - приоритет блока, выше или ниже остальных блоков
- `$callback_args` - аргументы для `$callback`
- `$post_obj` - объект записи
- `get_post_meta( $post_obj -> ID, 'po-news', true )` - true получаем последнее значение этого поля
- `$name_brand = $name_brand ? $name_brand : 'Общий';` если у записи у мета-поля нет значения то вставляем по-умолчанию 

Чтобы начать использовать пользовательские поля, нужно:
- в каком-нибудь типе данных (например запись или страница), при её создании или редактировании, нажать сверху на "Настройки экрана"
- поставить галочку возле надписи "Произвольные поля"
- внизу под записью появится мета-бокс "Произвольные поля"

У произвольных полей есть имя и значение. В значении может храниться какой-либо один контент (текст, число, изображение и т.д.). После создания произвольного поля, его можно выбирать в выпаающем списке во время создания или редактирования поста/записи.

## Темизация произвольных полей
Для того чтобы использовать произвольные поля в темизации WordPress, нужно в коде цикла разместить функцию `<?php the_meta(); ?>`. По-умолчанию, произвольные поля отображаются как маркированный список.

## Ссылки
- https://wp-kama.ru/function/add_meta_box
- https://wp-kama.ru/function/get_post_meta
- https://wp-kama.ru/handbook/codex/metadata
- https://wp-kama.ru/function/add_post_meta
- https://wp-kama.ru/function/register_meta
- **Плагин:** https://wordpress.org/plugins/advanced-custom-fields/ `Advanced Custom Fields` (Группы полей)
