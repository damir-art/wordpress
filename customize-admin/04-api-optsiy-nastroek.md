# API опций настроек
https://wp-kama.ru/id_3773/api-optsiy-nastroek.html

**API опций настроек** позволяют создавать поля форм (опции), в существующие страницы админки или страницы админки созданные пользователем (например для страниц плагина).

Для хранения опций в WordPress имеется отдельная таблица.

## Функции API
Регистрация/удаление опций
- register_setting()
- unregister_setting()

Добавление блоков и отдельных полей
- add_settings_section()
- add_settings_field()

Вывод на экран
- settings_fields()
- do_settings_sections()
- do_settings_fields()

Ошибки
- add_settings_error()
- get_settings_errors()
- settings_errors()

`register_setting()` и остальные функции опций вызываются в момент срабатывания хука `admin_init`.

Функцию реистрации опции можно поместить в функцию `legioner_add_admin_page()`.

Все три функциирегистрации опции, секции и поля взаимосвязаны.

Код страницы с опциями (опции обернуты в таблицу и лейбелом (лейбел можно не делать)):

    /**
    * Создание страницы в меню
    */
    function legioner_add_admin_page() {
      // add_menu_page( 'Моя страница в админке', 'Моя страница', 'manage_options', 'legioner-options', 'legioner_render_page' );
      // стили и скрипты подключающиеся для всей админки
      // add_action( 'admin_enqueue_scripts', 'legioner_admin_scripts' );

      $hook_suffix = add_menu_page( 'Моя страница в админке', 'Секция', 'manage_options', 'legioner-options', 'legioner_render_page' );
      // стили и скрипты подключающиеся для этой страницы
      add_action( "admin_print_scripts-{$hook_suffix}", 'legioner_admin_scripts' );

      // Делаем разные названия для секции и главной страницы в меню админки
      add_submenu_page( 'legioner-options', 'Моя страница в админке', 'Моя страница', 'manage_options', 'legioner-options', 'legioner_render_page' );

      // Создаём подстраницу
      add_submenu_page( 'legioner-options', 'Моя подстраница в админке', 'Моя подстраница', 'manage_options', 'legioner-options-subpage', 'legioner_render_subpage' );

      // К хуку прикрепляем функцию регистрирации опции (настройки)
      add_action( 'admin_init', 'legioner_register_setting' );

    }
    add_action( 'admin_menu', 'legioner_add_admin_page' );

    // Отрисовка страницы
    function legioner_render_page() {
      require get_template_directory() . '/inc/admin/templates/legioner-options.php';
    }

    // Отрисовка подстраницы
    function legioner_render_subpage() {
      require get_template_directory() . '/inc/admin/templates/legioner-options-subpage.php';
    }

    // Подкючаем стили и скрипты для админки
    function legioner_admin_scripts() {
      wp_enqueue_style( 'legioner-admin-style', get_template_directory_uri() . '/inc/admin/style.css' );
    }

    // Функция регистрирации опции (настройки)
    function legioner_register_setting() {
      // регистрируем группу опций (для страницы)
      register_setting(
        'legioner_group_general',
        'legioner_general_email'
      );
      // регистрируем секцию опций (для секции)
      add_settings_section(
        'legioner_section_general',
        'Секция опций',
        function () {
          echo '<p>Описание секции</p>';
        },
        'legioner-options'
      );
      // регистрируем поле опции (для опции)
      add_settings_field(
        'legioner_field_email',
        'Емаил',
        'legioner_general_field_email',
        'legioner-options',
        'legioner_section_general',
        array( 'label_for' => 'legioner_field_email' ),
      );
    }

    // Функция создающаяя HTML-код поля
    function legioner_general_field_email() {
      echo '<input class="regular-text" type="email" name="legioner_general_email" id="legioner_field_email" />';
    }

Код шаблона страницы:

    <!-- CSS-классы WordPress см. страницы админки -->
    <div class="wrap">
      <h1>Моя страница</h1>
      <form action="options.php" method="post">
        <?php
          settings_fields( 'legioner_group_general' ); // отрисовка скрытых полей формы
          do_settings_sections( 'legioner-options' ); // отрисовка всех опций, групп, секций, полей
          submit_button(); // отрисовка кнопки, сохраняет изменения опций
        ?>
      </form>
    </div>
