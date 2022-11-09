# Customize
Настройки темы из админки. В основном нужен для премиальных тем.

`Админка > Внешний вид > Настроить`  
https://wp-kama.ru/handbook/theme/customize-api  
https://developer.wordpress.org/themes/customize-api/  
https://developer.wordpress.org/reference/classes/page/5/  
Кастомайзер WordPress - интерфейс для настройки разных опций темы.  
Код размещают в functions.php, обычно подключают отдельные файлы `require __DIR__ . '/functions/file.php'`

Поддержка кастомайзера осуществляется через `add_theme_support()`, также можно создавать пользовательские настройки.

## Оглавление
add_theme_support():
- custom-logo
- custom-background
- custom-header

Пользовательские настройки (опции), customize API:
- customize API

## Функции
- get_theme_mod() - получить значение указанной опции настройки темы
