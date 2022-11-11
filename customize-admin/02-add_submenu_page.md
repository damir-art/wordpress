# add_submenu_page()
https://wp-kama.ru/function/add_submenu_page  
Добавляем подраздел в админку.

Код создающий страницу, подстраницу и разные названия для секции и главной страницы, также вынесли HTML-код страниц в отдельные файлы:

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

    // Отрисовка подстраницы
    function legioner_render_subpage() {
      ?>
      
      <!-- CSS-классы WordPress см. страницы админки -->
      <div class="wrap">
        <h1>Моя подстраница</h1>
      </div>
      
      <?php
    }

    // Подкючаем стили и скрипты для админки
    function legioner_admin_scripts() {
      wp_enqueue_style( 'legioner-admin-style', get_template_directory_uri() . '/inc/admin/style.css' );
    }
