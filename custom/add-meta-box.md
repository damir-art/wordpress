# Мета боксы
Произвольные поля, пользовательские поля.  
Для записей и таксономий, можно добавлять свои пользовательские поля.

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