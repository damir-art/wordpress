# Wordpress
Руководство Wordpress (черновые записи).

**WordPress** - самая популярная система управления сайтом в мире. WordPress - система управления контентом, любым контентом в любом виде.

По данным w3techs.com, 40% сайтов в мире используют CMS WordPress.

## Что можно создавать на WordPress
Для WordPress созданы тысячи тем (на различную тематику) и плагинов (модулей), с помощью которых можно создать абсолютно любой сайт, начиная от простого сайта визитки и заканчивая корпоративными порталами и интернет магазинами.

- блоги и новостные сайты
- каталоги товаров, недвижимости, автомобилей и других сущностей
- интернет-магазины с корзиной и доставкой
- одностраничники, лендинги, продающие и промо страницы
- корпоративные сайты
- форумы, социальные сети
- системы бронирования и оформления заказов
- сайты купонов, скидок, групповых покупок
- системы многоуровневого доступа с реферальной системой
- сайты объявлений, вакансий
- видеохостинг наподобие youtube
- сайт вопросов и ответов FAQ, обратной связи, помощи
- любой вид сервиса
- CRM
- и многое многое другое

## Об этом руководстве
В этом руководстве по WordPress рассмотрены следующие вопросы:
- установка WordPress на домашний компьютер (локально)
- установка вордпрес в интернете (на хостинг)
- изучение административной панели WordPress
- изучение Gutenberg
- начальная настройка и оптимизация производительности WordPress
- установка и изучение различных готовых тем
- установка и изучение частоиспользуемых плагинов WordPress
- улучшение производительности WordPress (снижение нагрузки на сервер, ускорение сайта)
- повышение безопасности WordPress
- обслуживание и администрирование WordPress (обновление ядра, сохранение/перенос базы данных и т.д.)
- SEO-оптимизация WordPress (продвижение вашего сайта и улучшение позиций в поиске),
- создание своей собственной темы
- создание своих типов записей, таксономий, мета полей, кастомных циклов, блоков Gutenberg и т.д.
- создание собственного виджета и плагина
- дополнительные уроки и статьи для общего развития

## Инструменты для работы с WordPress
Прежде чем начать создавать сайты на Wordpress вам понадобится веб-сервер. Для того чтобы создать сайт у себя на компьютере вы можете установить себе локальный веб-сервер **Open Server** https://ospanel.io

Чтобы создать сайт и разместить его в интернете вам понадобится хостинг, например Бегет. Там же можно получить и бесплатный хостинг.

Помимо локального веб-сервера, вам также понадобится текстовый редактор или IDE. Я использую текстовый редактор для программистов: **Visual Studio Code**.

Плагины для VS Code:
- PHP IntelliSense
- WordPress Snippets

## Разное
- когда пишите свои функции, например в файле `functions.php` вставляйте перед её именем имя темы, например `function theme_foo() {}`, это необходимо во избежании конфликта имён функций
- при разработке WordPress в инспекторе браузере во влкадке `Сеть`, отключайте кэш

Первая функция в function.php, должна быть функция `themeName_debug()` форматированно выводящая объекты и массивы:

    /**
    * Показать массив, объект
    */
    function themeName_debug( $elem ) {
      echo '<pre>';
      print_r( $elem );
      echo '</pre>';
    }

или:

    function legioner_debug( $data ) {
      echo '<pre>' . print_r( $data, 1 ) . '</pre>';
    }

Отмена сжатия изображений при загрузке:

    /**
    * Отменяем сжатие изображений
    */
    add_filter( 'jpeg_quality', function( $quality ) {
      return 100;
    });

Разное:
- `show current template` - (рекомендуется) плагин показывающий какой шаблон сейчас используется
- `which template` - плагин показывающий какой шаблон сейчас используется
- `which template file` - плагин показывающий какой шаблон сейчас используется
