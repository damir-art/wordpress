# bloginfo()
https://wp-kama.ru/function/bloginfo  
Вывод информации о сайте. Является тегом шаблона и может быть использован в любом месте шаблона.

Данные могут храниться в админке, например:
- `Настройки > Общие`

Список параметров:
- `bloginfo('name')` - название сайта
- `bloginfo('description')` - описание блога
- `bloginfo('charset')` - кодировка сайта, UTF-8
- `bloginfo('template_url')` - URL путь к теме, https://site.name/wp-content/themes/theme-name
  - `bloginfo('stylesheet_directory')` - тоже самое
  - `bloginfo('template_directory')` - тож самое
  - `echo get_stylesheet_directory_uri()` - тоже самое (рекомендуется)
  - `echo get_template_directory_uri()` - тоже самое
- `bloginfo('stylesheet_url')` - URL путь к style.css, https://site.name/wp-content/themes/theme-name/style.css
- `bloginfo('language')` - локаль сайта, ru-RU
- `language_attributes()` - html lang="ru-RU"
- `bloginfo('html_type')` - text/html или другой тип (Content-Type)
- `echo get_template_directory()` - файловый путь к теме D:\server\domains\site.name/wp-content/themes/theme_name
- `bloginfo('admin_email')` - email администратора
- `bloginfo('url')` - URL на главную страницу сайта https://site.name
  - `bloginfo('wpurl')` - тоже самое
  - `bloginfo('home')` - тоже самое
  - `bloginfo('siteurl')` - тоже самое
  - `echo home_url()` - рекомендуется
  - `echo site_url()` - рекомендуется

Еще есть параметры связанные с RSS, пингами, версией и т.п.
