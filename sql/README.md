# MySQL в WordPress
Работаем с базой данный MySQL в WordPress, напрямую. При установке WordPress в БД создаётся 12 таблиц.

Класс `wpdb{}` - позволяет производить любые операции с базой данных WordPress: вставлять, обновлять, получать, удалять данные.

К методам класса `wpdb{}` нужно обращаться через глобальную переменную `$wpdb` (это экземпляр класса). При использовании внутри пользовательских функций, переменную нужно глобализовать: `global $wpdb;`.

Выводим все данные объекта, также можно выведутся названия всех таблиц:

    echo '<pre>';
    print_r($wpdb);
    echo '</pre>';

С помощью методов `$wpdb` можно управлять таблицами и данными в БД:

    global $wpdb;

    // Выбрать все заголовки из таблицы wp_posts
    $result = $wpdb->get_results( "SELECT post_title FROM wp_posts" );

    echo '<pre>';
    print_r($result); // массив объектов
    echo '</pre>';

    // Выбрать все заголовки из таблицы wp_posts, столбца 'post'
    $result = $wpdb->get_results( "SELECT post_title FROM wp_posts WHERE post_type = 'post'" );

    foreach($result as $item ) {
      echo $item->post_title . '<br />';
    }

## Схема запросов к БД
- `global $wpdb;`
- `$done = $wpdb->query( "UPDATE ..." );` - произвольный запрос к БД
- GET data - получаем данные
  - `$objects = $wpdb->get_results( "SELECT ..." );` - получить несколько строк таблицы
  - `$object = $wpdb->get_row( "SELECT ..." );` - получить строку таблицы
  - `$values = $wpdb->get_col( "SELECT ..." );` - получить столбец таблицы
  - `$var = $wpdb->get_var( "SELECT ..." );` - получить ячейку таблицы
  - `$var = $wpdb->get_var( "SELECT ..." );` -
- CRUD data - создание (create, insert), чтение (read, select), модификация, редактирование (update, update), удаление (delete)
  - `$done = $wpdb->insert( 'table', [ 'column' => 'foo', 'field' => 'bar' ] );` - вставка новой строки в таблицу
  - `$done = $wpdb->update( 'table', [ 'column' => $_GET['val'] ], [ 'ID' => 1 ] );` - обновление строки в таблице
  - `$done = $wpdb->delete( 'table', [ 'ID' => 1 ] );` - удаление строки из таблицы
  - `$done = $wpdb->replace( 'table_name', [ 'ID' => 1, 'column' => $_GET['val'] ] );` - замена строки в таблице
- ДОПОЛНИТЕЛЬНО
  - `prepare` - защита запроса от SQL инъекций
  - `esc_like` - очистка LIKE строки
  - `show/hide/print_error` - показать или спрятать ошибки SQL
  - `get_cool_info` - получить информацию о колонке
  - `flush` - сброс кеша

## Разное
Создавать свои таблицы рекомендуется с помощью функции `dbDelta()`. Или можно использовать обычный SQL запрос и метод `$wpdb->query('CREATE TABLE ...');`.

Для плагинов создание новых таблиц обычно вешается на хук активации плагина: `register_activation_hook()`.

Еще про CRUD - в системах, реализующих доступ к базе данных через API в стиле REST, эти функции реализуются зачастую (но не обязательно) через HTTP-методы PUT, POST, GET, PATCH, DELETE:

    CRUD    HTTP
    Create  POST, PUT (if we have id or uuid)
    Read    GET
    Update  PUT
    Delete  DELETE
