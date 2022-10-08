# get_terms()

    $terms = get_terms([
      'taxonomy' => 'post_tag', // название таксономии или их список
      'hide_empty' => false,
    ]);

    echo '<pre>';
    echo print_r($terms);
    echo print_r($terms->name); // получаем доступ к свойству объекта
    echo '</pre>';


    $brand->name

Каждый термин хранит это объект и хранит он в себе следующие свойства:

    WP_Term Object (
      [term_id] => 28
      [name] => Ameri
      [slug] => ameri
      [term_group] => 0
      [term_taxonomy_id] => 28
      [taxonomy] => am_product_brand
      [description] => 
      [parent] => 0
      [count] => 8
      [filter] => raw
    )

Что возвращает `get_terms()`:
- массив объектов WP_Term - при успешном получении
- array() (пустой массив) - если термины не найдены
- WP_Error - если любой из указанных таксономий не существует
- количество найденных терминов (в виде строки) - если fields = count.

С помощью фильтров запрос перед отправкой можно изменять и управлять выводом:
- get_terms - фильтрует результат (найденные термины)
- list_terms_exclusions - исключает термины (получает список всех исключённых терминов)
- get_terms_orderby - фильтрует ORDER BY часть запроса

Является основой для:
- wp_nav_menu_item_taxonomy_meta_box(),
- wp_count_terms(),
- wp_get_nav_menus(),
- wp_get_object_terms(),
- get_tags(),
- get_term_by(),
- get_all_category_ids(),
- get_categories()

## Разное
- функция get_terms() является обёрткой для класса WP_Term_Query{}: https://wp-kama.ru/function/wp_term_query
- поддерживает запросы для метаданных (произвольных полей), работает также как meta_query в wp_query
