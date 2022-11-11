# Вывод опций на сайте
https://wp-kama.ru/function/get_option

`get_option()` - обращение к значению опции (настройки)

Список предустановленных опций:
- admin_email - E-mail администратора блога
- blogname - название блога. Устанавливается в настройках.
- blogdescription - описание блога. Устанавливается в настройках.
- blog_charset - Кодировка блога. Устанавливается в настройках.
- date_format - формат даты. Устанавливается в настройках.
- default_category - категория постов по умолчанию. Устанавливается в настройках.
- home - Адрес домашней страницы блога. Устанавливается в основных настройках.
- siteurl - Адрес WordPress. Устанавливается в основных настройках *(siteurl отличается от get_bloginfo('siteurl') (который возвращает url домашней страницы блога). И не отличается от get_bloginfo('wpurl'))*
- template - название текущей темы.
- start_of_week - день с которого начинается неделя. Устанавливается в основных настройках.
- upload_path - каталог загрузки по умолчанию. Устанавливается в настройках.
- posts_per_page - максимальное число постов на странице. Устанавливается в настройках чтения.
- posts_per_rss - максимальное число постов выводимых в фид. Устанавливается в настройках чтения.

## Вывод значения опции в админке страницы

    // Функция создающаяя HTML-код поля
    function legioner_general_field_email() {
      $legioner_general_email = esc_attr(get_option( 'legioner_general_email' )); // получаем значение настройки, добавляем его в атрибут value

      echo '<input class="regular-text" type="email" name="legioner_general_email" id="legioner_field_email" value="' . $legioner_general_email . '" />';
    }

Вводим емаил и жмем на кнопку `Сохранить изменения`.

## Вывод значения опции на сайте
В шаблоне сайта прописываем:

    echo get_option( 'legioner_general_email' );

## Вывод информационных сообщений
Выввод информационных сообщений `<?php settings_errors(); ?>` об ошибке или успешном сохранении после изменения значения опции в админке.

    <!-- CSS-классы WordPress см. страницы админки -->
    <div class="wrap">
      <h1>Моя страница</h1>

      <?php settings_errors(); ?>

      <form action="options.php" method="post">
        <?php
          settings_fields( 'legioner_group_general' );  // отрисовка скрытых полей формы
          do_settings_sections( 'legioner-options' ); // отрисовка всех опций, групп, секций, полей
          submit_button(); // отрисовка кнопки, сохраняет изменения опций
        ?>
      </form>
    </div>
