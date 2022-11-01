# Customize API
`Админка > Внешний вид > Настроить`  
Customize API - создаёт функционал, предварительного просмотра изменений WordPress на фронте, в реальном времени, без перезагрузки страницы. Интерфейс для настройки темы из админки: текст, цвет, поля, виджеты, меню и мн.др. Часто используется для создания премиальных тем.

Объекты кастомайзера:
- панель (panel) - объединяет секции
- секция (section) - объединяет элементы цправления (может существовать вне панели)
  - по-умолчанию, в темах есть предустановленные секции
- элемент управления (control) - текстовое поле, радиокнопка, выпадающий список и т.п. (не может существовать вне секции)
- настройка (setting) - настройки кастомайзера связывают control с настройкой (хранится в БД), control управляет настройкой

Панель от секции отличается тем, что внутри панели содержатся секции, например в настройках темы, `Свойства сайта` это секция, а `Меню` это панель.

## Код
При создании настроек тем, мы работаем с методами объекта `WP_Customize_Manager()` и хуком `customize_register`:

    add_action( 'customize_register', 'legioner_customize_register' );

    function legioner_customize_register( $wp_customize ) {
      // Здесь делаем что-либо с $wp_customize - объектом класса WP_Customize_Manager, например

      // Действия с панелями
      $wp_customize->add_panel();     // добавить панель
      $wp_customize->get_panel();     // получить панель
      $wp_customize->remove_panel();  // удалить панель

      // Действия с секциями
      $wp_customize->add_section();    // добавить секцию
      $wp_customize->get_section();    // получить секцию
      $wp_customize->remove_section(); // удалить секцию

      // Действия с настройками
      $wp_customize->add_setting();    // добавить настройку
      $wp_customize->get_setting();    // получить настройку
      $wp_customize->remove_setting(); // удалить настройку

      // Действия с элементами управления
      $wp_customize->add_control();    // добавить элемент управления
      $wp_customize->get_control();    // получить элемент управления
      $wp_customize->remove_control(); // удалить элемент управления
    }

## Добавим свою настройку
Добавим настройку `Цвет ссылок` в секцию `Цвета`:

    function legioner_customize_register( $wp_customize ) {

      // Добавление настройки
      $wp_customize->add_setting(
        // 'header_textcolor', // можно передать строку или WP_Customize_Setting
        'legioner_link_color', // можно передать свою строку, id
        array(
          'default'   => '#ff0000',
          'transport' => 'refresh', // перезагружает страницу после изменений, 'postMessage' работает как AJAX без перезагрузки (но нужно будет дополнительно написать JS-код)
        )
      );

      // Добавление control
      $wp_customize->add_control(
        new WP_Customize_Color_Control(
          $wp_customize, // WP_Customize_Manager
          'legioner_link_color', // Setting id
          array( // Args, including any custom ones.
            'label'    => 'Цвет ссылок',
            'section'  => 'colors', // ID секции
            // 'settings' => 'legioner_link_color'
          )
        )
      );
    }
    add_action( 'customize_register', 'legioner_customize_register' );

По умолчанию цвет ссылок красный, можно в настрйоках темы установить свой, цвета и другие настройки хранятся в БД.

Посмотреть значения настроек можно через функции:
- `get_theme_mod()` - получает значение указанной опции (настройки) текущей темы
- `get_theme_mods()` - получает все значения кастомайзера

Получим все настройки кастомайзера:

    legioner_debug(get_theme_mods());

Получим цвет ссылок:

    legioner_debug(get_theme_mods()['legioner_link_color']);
    legioner_debug(get_theme_mod('legioner_link_color'));

Добавляем цвет ссылки на сайт, генерируем CSS который обновит цвета ссылок:

    /**
    * Добавляем цвет ссылок из настроек
    */
    function legioner_customize_css() {
      ?>
        <style type="text/css">
          a { color: <?php echo get_theme_mod('legioner_link_color', '#ff0000'); ?>; }
        </style>
      <?php
    }
    add_action( 'wp_head', 'legioner_customize_css');

## Секции WordPress
Список предустановленных секций.

    Title                 ID                 Priority (Order)
    Site Title & Tagline  title_tagline      20
    Colors                colors             40
    Header Image          header_image       60
    Background Image      background_image   80
    Menus (Panel)         nav_menus          100
    Widgets (Panel)       widgets            110
    Static Front Page     static_front_page  120
    default                                  160
    Additional CSS        custom_css         200

ID секции можно посмотреть в HTML-коде, например `Свойства сайта` это `li` с `id` accordion-section-`title_tagline`.

Можно создавать свои секции.
