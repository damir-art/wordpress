# Разное
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
