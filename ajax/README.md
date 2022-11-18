# AJAX в WordPress
https://wp-kama.ru/id_2018/ajax-v-wordpress.html  

С AJAX в WordPress работают в двух режимах:
- через админку (авторизованные пользователи), через хук `wp_ajax_(action)`
- публично (посетители сайта), через хук `wp_ajax_nopriv_(action)`

`(action)` - придумываем сами, куда именно Wordpress направит AJAX запрос, на какой обработчик  
Все AJAX-запросы должны идти на один адрес: `ajaxurl`  
`var ajaxurl = '/wp-admin/admin-ajax.php'`  
В админке `ajaxurl` уже определён.  
В публичной части `ajaxurl` определять нужно отдельно, чтобы сайт нормально переносился с одного хостинга на другой, чтобы он не зависел от домена.  
Файл `admin-ajax.php` сам определяет на какой action отправить AJAX-запрос.  
В AJAX-запросе мы передаём action.  

Код AJAX-запроса:

    jQuery(document).ready(function($) {

      // AJAX-запрос
      $('#main_post').autocomplete({ // работаем с UI autocomplete
        source: ajaxurl + '?action=main_post_action', // куда отправляется запрос
        minLength: 2, // сколько символов должен набрать пользователь, перед отправкой запроса
        delay: 500, // задержка перед отравкой запроса при наборе символов
        select: function(event, ui) { // событие которое выполнится, когда придет ответ на AJAX-запрос, автокомплит покажет выпадающий список, когда выберем из списка пост
          $('#main_post_id').val( ui.item.id ); // ID выбранного поста добавится в скрытое поле
        }
      });

    });


Здесь `(action)` это `main_post_action`.

Обработчик AJAX-запроса:

    /**
    * Обработчик AJAX-запроса, формируем ответ,
    * Вместо отдельной функции используем колбэк
    */
    add_action( 'wp_ajax_main_post_action', function() {

      $main_post_s = $_GET['term']; // term ключ от autocomplete
      // Создаём свой массив для проверки работы запроса
      $result = [
        [
          'label' => 'Post 1',
          'id' => 1
        ],
        [
          'label' => 'Post 2',
          'id' => 2
        ]
      ];
      // Переделываем массив в JSON-формат
      echo json_encode($result);
      // Завершаем вфполнение скрипта
      wp_die();
    });
