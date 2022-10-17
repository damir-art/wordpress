# Создаём каркас плагина
Создаём один из основных классов плагина: `/wp-content/plugins/banner/includes/class-banner.php`

В файле `class-banner.php` помещаем следующий код:

    class Banner {
      // Создаём конструктор
      public function __construct() {
        // Создадим лог файла, вместо echo для проверки работы класса
        file_put_contents( BANNER_PLUGIN_DIR . 'log.txt', "Работает!\n", FILE_APPEND );
      }
    }

Каждый раз при обновлении страницы, будет отрабатывать плагин и создаваться объект класса `BANNER`, также в логах будет появляться дополнительнся запись.

В конструкторе нужно разместить методы которые будут создавать экземпляры классов админки и публичной части.

    // Метод подключающий классы для работы с админкой и публичной частью
    private function load_dependecies() {
      require_once BANNER_PLUGIN_DIR . 'admin/class-banner-admin.php';
      require_once BANNER_PLUGIN_DIR . 'public/class-banner-public.php';
    }

Создаём файлы этих классов.

## admin/class-banner-admin.php

    class Banner_Admin {
      // Конструктор
      public function __construct() {
        // Создадим лог файла, вместо echo для проверки работы класса
        file_put_contents( BANNER_PLUGIN_DIR . 'log.txt', "Admin\n", FILE_APPEND );
      }
    }


## public/class-banner-public.php

    class Banner_Public {
      // Конструктор
      public function __construct() {
        // Создадим лог файла, вместо echo для проверки работы класса
        file_put_contents( BANNER_PLUGIN_DIR . 'log.txt', "Public\n", FILE_APPEND );
      }
    }

## includes/class-banner.php

    class Banner {
      // Конструктор
      public function __construct() {
        // Создадим лог файла, вместо echo для проверки работы класса
        file_put_contents( BANNER_PLUGIN_DIR . 'log.txt', "Banner\n", FILE_APPEND );
        // Вызываем приватный метод load_dependecies()
        $this->load_dependecies();
        $this->define_admin_hooks();
        $this->define_public_hooks();
      }

      // Метод подключающий классы для работы с админкой и публичной частью
      private function load_dependecies() {
        require_once BANNER_PLUGIN_DIR . 'admin/class-banner-admin.php';
        require_once BANNER_PLUGIN_DIR . 'public/class-banner-public.php';
      }

      // Создаём экземпляр класса Banner_Admin
      private function define_admin_hooks() {
        $plugin_admin  = new Banner_Admin;
      }

      // Создаём экземпляры класса Banner_Public
      private function define_public_hooks() {
        $plugin_public  = new Banner_Public;
      }
    }

## banner/banner.php
В файле `banner.php` создаём функцию (рекомендуется, чтобы в шлобальной области видимости было меньше переменных), которая создаёт объект от этого класса, далее вызываем эту функцию:

    // Подключаем класс banner.php
    require_once BANNER_PLUGIN_DIR . 'includes/class-banner.php';
    function run_banner() {
      // Создаём экземпляр класса Banner
      $plugin = new Banner;
    }
    run_banner();

## Дополнительно
В файл `/includes/class-banner.php` создаём метод, который вызывает хуки, которые должны отрабоать первоначально, до паблика и админки, например файлы локализации:



## Итого
Все файлы и код получившиеся до этого момента, можно использовать как начальный шаблон практически для любого плагина.

Сначала вызывается главный файл плагина `/banner.php`, который содержит в себе
- заголовки
- проверка на прямой доступ
- константы
- подключение класса активации плагина (создание таблицы)
- подключение основного класса плагина `/includes/class-banner.php` (создание экземпляра)

Экземпляр (объект) класса `/includes/class-banner.php`, содержит в себе:
- конструктор вызывающий приватные методы, который подключает:
  - классы админской и публичной части
  - создает экземпляр класса админской части
  - создает экземпляр публичной части
