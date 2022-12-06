# register_rest_route()
Регистрирует маршрут REST API и его эндпоинты (конечные точки). Регистрирует URL по которому будет срабатывать указанная PHP функция.

- создадим маршрут по адресу `domain/wp-json/myplugin/v1/author-posts/1`
- перейдя по ней, выведется JSON-ответ
- маршрут регистрируется на хуке `rest_api_init`
- пространство имён маршрута `myplugin/v1`, у WordPress `wp/v2`
  - имя плагина/темы уникально и пространство имён должно быть уникальным
- JSON-ответ возвращается функцией `my_awesome_func()`
- `/author-posts/(?P<id>\d+)` - маршрут указан как регулярное выражение для определения числа, чтобы по URL /author-posts/123 нам выдавались посты автора с ID 123
- `domain/wp-json/myplugin/v1/author-posts/1` - конечная точка

Пример:

    // создание пользовательского маршрута
    add_action( 'rest_api_init', function() {
      // пространство имен
      $namespace = 'myplugin/v1';
      // маршрут
      $rout = '/author-posts/(?P<id>\d+)';
      // параметры конечной точки (маршрута)
      $rout_params = [
        'methods'  => 'GET',
        'callback' => 'my_awesome_func',
        'args'     => [
          'arg_str' => [
            'type'     => 'string', // значение параметра должно быть строкой
            'required' => true,     // параметр обязательный
          ],
          'arg_int' => [
            'type'    => 'integer', // значение параметра должно быть числом
            'default' => 10,        // значение по умолчанию = 10
          ],
        ],
        'permission_callback' => function( $request ){
          // только авторизованный юзер имеет доступ к эндпоинту
          return is_user_logged_in();
        },
      ];
      register_rest_route( $namespace, $rout, $rout_params );
    });

    // функция обработчик конечной точки (маршрута)
    function my_awesome_func( WP_REST_Request $request ){
      $posts = get_posts( [
        'author' => (int) $request['id'],
      ] );
      if ( empty( $posts ) ) {
        return new WP_Error( 'no_author_posts', 'Записей не найдено', [ 'status' => 404 ] );
      }
      return $posts;
    }

При регистрации произвольных маршрутов рекомендуется указывать пространство имен.
