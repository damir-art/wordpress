# Страница админской части плагина
Создание страницы плагина в админке. Страница плагина в админке, обычно используется для настройки плагина или добавления данных.

Для этого используются API опций (API настроек WordPress).  
`admin_menu` - хук срабатывает перед загрузкой меню администрирования в админке.  
`array` - используется в ООП стиле.  
`admin_menu` - метод, имя метода в ООП стиле может совпадать с именем хука потому что он виде только внутри класса.  
`add_menu_page()` - добавляет пункт (страницу) верхнего уровня в меню админки.  
`__()` - функция для файлов переводов, все слова сначала пишут на английском.  
`banner` - text domain, совпадает с названием плагина.  
`manage_options` - администратор имеет право доступа к этой странице.  
`render_page` - метод отрисовывающий страницу.  

    class Banner_Admin {
      // Конструктор
      public function __construct() {
        // Создадим лог файла, вместо echo для проверки работы класса
        // file_put_contents( BANNER_PLUGIN_DIR . 'log.txt', "Admin\n", FILE_APPEND );
        // admin_menu - хук срабатывает перед загрузкой меню администрирования в админке
        add_action( 'admin_menu', array( $this, 'admin_menu' ) );
      }

      // метод должен быть публичным, потому что это колбэк
      public function admin_menu() {
        // создаём страничку для админки
        add_menu_page( __('Banner', 'banner'), __('Banner', 'banner'), 'manage_options', 'banner', array( $this, 'render_page' ), 'dashicons-admin-comments' );
      }

      // метод отрисовывающий страницу
      public function render_page() {
        // echo '<h1>Заголовок</h1>';
        require_once BANNER_PLUGIN_DIR . 'admin/templates/banner.php';
      }
    }

## Шаблон для страницы
Создаём шаблон для страницы в админке.  
Создаем папку `/admin/templates/banner.php`:

    <div class="wrap">
      <h1><?php _e('Заголовок', 'banner') ?></h1>
    </div>
