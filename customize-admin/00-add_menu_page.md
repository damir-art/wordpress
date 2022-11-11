# add_menu_page()
Добавляет пункт меню (новую страницу) верхнего уровня в меню админ-панели (в один ряд с постами, страницами, пользователями и т.д.).

Пункт меню добавляется через хук `admin_menu`.

Код добавления страницы в админку, HTML-кода и CSS-файла для этой страницы:

    /**
    * Создание страницы в меню
    */
    function legioner_add_admin_page() {
      // add_menu_page( 'Моя страница в админке', 'Моя страница', 'manage_options', 'legioner-options', 'legioner_render_page' );
      // стили и скрипты подключающиеся для всей админки
      // add_action( 'admin_enqueue_scripts', 'legioner_admin_scripts' );

      $hook_suffix = add_menu_page( 'Моя страница в админке', 'Моя страница', 'manage_options', 'legioner-options', 'legioner_render_page' );
      // стили и скрипты подключающиеся для этой страницы
      add_action( "admin_print_scripts-{$hook_suffix}", 'legioner_admin_scripts' );
    }
    add_action( 'admin_menu', 'legioner_add_admin_page' );

    // Отрисовка страницы
    function legioner_render_page() {
    ?>

    <!-- CSS-классы WordPress см. страницы админки -->
    <div class="wrap">
      <h1>Моя страница</h1>
    </div>

    <?php
    }

    // Подкючаем стили и скрипты для админки
    function legioner_admin_scripts() {
      wp_enqueue_style( 'legioner-admin-style', get_template_directory_uri() . '/inc/admin/style.css' );
    }
