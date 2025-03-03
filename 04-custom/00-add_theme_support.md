# add_theme_support()
http://wp-kama.ru/function/add_theme_support

**add_theme_support()** - функция позволяет регистрировать темам и плагинам, поддержку новых воможностей WordPress.

    add_theme_support( 'post-thumbnails' );      // поддержка миниатюр
    add_theme_support( 'menus' );                // поддержка меню (создается автоматически через register_nav_menu())
    add_theme_support( 'custom-background' );    // изменение фона сайта из админки
    add_theme_support( 'custom-logo' );          // изменение логотипа через админку
    add_theme_support( 'custom-header' );        // изменение изображения в шапке сайта из админки
    add_theme_support( 'automatic-feed-links' ); // ссылки на RSS-фиды постов и комментариев
    add_theme_support( 'html5' );                // внедрение тегов html5 в код WordPress по-умолчанию, например в поисковую строку
    add_theme_support( 'title-tag' );            // поддержка title, нужно удалить данный тег в header
    add_theme_support( 'widgets' );              // поддержка виджетов, создается автоматически при использовании register_sidebar()

HTML5:

    add_theme_support('html5', array(
      'comment-list',
      'comment-form',
      'search-form',
      'gallery',
      'caption',
      'script',
      'style',
    ));

## Пример

    <?php
      // Начальные настройки сайта
      add_action( 'after_setup_theme', 'site_setup' );

      function site_setup() {
        add_theme_support( 'custom-logo' );
        add_theme_support( 'title-tag' );
        add_theme_support( 'post-thumbnails' );
      }

## Работаем с title

    add_theme_support('title-tag');
    add_filter('document_title_separator',
      function() {
        return '&amp;raquo;';
      }
    );
