# Активация удаление плагина
Плагин можно писать в функциональном стиле или ООП. Напишем плагин в ООП стиле и создадим структуру файлов плагина.

Структура файлов и папок плагина:

    admin/
    includes/
      class-banner-activate.php
    public/
    banner.php
    index.php

## includes/

### class-banner-activate.php
Класс срабатывает один раз при активации плагина:

    class Banner_Activate {
      // объявляем статичный метод чтобы вызывать его без создания экземпляра
      public static function activate() {
        // создаём таблицу в БД, если она не существует
      }
    }

Класс может проверять существуют ли таблицы необходимые для плагина, если нет то создавать их. Получить код создания таблицы, проще в PHPMyAdmin, создать там таблицу и скопировать получившийся код, затем эту таблицу нужно удалить, пусть её создаёт плагин.

- переходим в PHPMyAdmin
- вводим имя таблицы `banner_content`
- вводим `2` столбца
- жмём по кнопке `Создать`
- Первый столбец:
  - имя `id`
  - тип `INT`
  - атрибуты `UNSIGNED`
  - ставим галочку `A_I`
  - автоматически должен выбраться `PRIMARY` в индекс
- Второй столбец:
  - имя `content`
  - тип `TEXT`
- жмём по кнопке `Сохранить` в PHPMyAdmin
- переходим на вкладку `Вставить`
- вводим в поле `content`: Контент 1
- жмем по кнопке `Вперед`

Копируем код и вставляем в виде комментария в файл `class-banner-activate.php`:

    INSERT INTO `banner_content` (`id`, `content`) VALUES (NULL, 'Контент 1')

Получаем код создания таблицы, если её нет:
- жмём по вкладке `Экспортировать`
- заголовок раздела `Экспорт строк из таблицы "banner_content"`
- метод экспорта: `Обычный - отображать все возможные настройки`
- вывод: `Отобразить вывод как текст`
- параметры формата: `Структура`
- параметры создания объектов, включить: `IF NOT EXISTS`
- жмем по кнопке `Экспорт`

Копируем код и вставляем в файл `class-banner-activate.php`:

    CREATE TABLE IF NOT EXISTS `banner_content` (
      `id` int(10) UNSIGNED NOT NULL AUTO_INCREMENT,
      `content` text COLLATE utf8mb4_unicode_ci NOT NULL,
      PRIMARY KEY (`id`)
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci

Удаляем таблицу `banner_content` в базе данных.

Добавляем в код запрос добавления тестовой записи. Тестовая запись должна быть добавлена безопасно. В итоге код в файле `class-banner-activate.php` будет выглядеть следующим образом:

    <?php
    class Banner_Activate {
      // объявляем статичный метод чтобы вызывать его без создания экземпляра
      public static function activate() {
        // создаём таблицу в БД, если она не существует
        global $wpdb;
        $wpdb->query( 
          "CREATE TABLE IF NOT EXISTS `banner_content` (
          `id` int(10) UNSIGNED NOT NULL AUTO_INCREMENT,
          `content` text COLLATE utf8mb4_unicode_ci NOT NULL,
          PRIMARY KEY (`id`)
          ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci"
        );
        // добавляем тестовую запись (%s - экранирование строк, prepare() - подготавливает SQL-запрос для выполнения)
        $query = "INSERT INTO `banner_content` (`id`, `content`) VALUES (NULL, %s)";
        $wpdb->query( $wpdb->prepare( $query, 'Контент 1' ) );
      }
    }

Проверяем плагин, активируем его, в БД должна появится таблица `banner_content` и тестовая запись.

При активации плагина, WordPress проверяет какие функции привязаны к хуку `register_activation_hook()`, в нашем случае привязанная функция запускает метод `Banner_Activate::activate();`.
