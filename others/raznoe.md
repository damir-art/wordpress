# Разное
* К одному посту рекомендуют цеплять одну рубрику, а меток сколько угодно.

**Перевод** - для мультиязычных тем, хранится в wp-content/languages/themes/ файлы `*.mo` и `*.po`

**Ошибки** - если что-то идет не так, то можно включить показ ошибок PHP, через .htaccess (php show error on) или конфиг WordPress (WP-DEBUG TRUE)

**Системные папки** - в них находятся файлы ядра WordPress и лучше их не менять: `/wp-admin/`, `/wp-includes/`.

## Стартовые темы
underscores.me
1. Пишем в поле название темы
2. Жмём кнопку Generate
3. В расширенных настройках можно указать `theme slug` он предназначен для добавления префикса в функциях темы
4. Удаляем файл стилей
5. Удаляем папки: languages, layouts,
6. Удаляем файлы: LICENSE, README

GeneratePress, Ocean WP, Astra

## Записи
**Записи** - в вордпресс, записи это все записи любого типа. В админке это только посты. Также и по архивам: есть архив-история, есть архив-категоризация (склад) `has_archive => true`.

**Формат записи** - это подключение разных типов шаблонов для поста. Формат записи можно выбрать справа при написании поста: стандартный, заметка, изображение, видео и т.д. Например в теме 17 они хранятся в template-parts/post. У страниц тоже можно создавать свои шаблоны, но это не форматы.

**Мета поля** - возможность создания дополнительных характеристик для записи. Стандартно это заголовок и текст поста. Мета поля могут быть: `title`, `description`, `keywords`, различные характеристики товара и т.п.

**Синхронизация** - локального сайта и сайта на хостинге, копать в сторону GIT с SSH доступом или миграция для БД.

## Зарезервированные названия в WordPress
Зарезервированные названия публичных и частных переменных.

    attachment
    attachment_id
    author
    author_name
    calendar
    cat
    category_name
    category__and
    category__in
    category__not_in
    comments_per_page
    comments_popup
    cpage
    day
    debug
    error
    exact
    feed
    hour
    link
    minute
    monthnum
    more
    name
    nav_menu
    nopaging
    offset
    order
    orderby
    p
    page
    paged
    pagename
    page_id
    pb
    perm
    post
    posts
    posts_per_archive_page
    posts_per_page
    post_format
    post_mime_type
    post_status
    post_type
    preview
    robots
    s
    search
    second
    sentence
    showposts
    static
    subpost
    subpost_id
    tag
    tag_id
    tag_slug__and
    tag_slug__in
    tag__and
    tag__in
    tag__not_in
    taxonomy
    tb
    term
    type
    w
    withcomments
    withoutcomments
    year

## Админка > Настройки > Постоянные ссылки > Простые
"Простые" означает работу с GET параметрами в адресной строке, движок их разбирает и решает, что нужно отобразить.

Поэтому:
- правила для ЧПУ в .htaccess отсутствуют, так как это не ЧПУ
- так как всё теперь работает на GET параметрах, то "виртуальный" robots.txt открывается по адресу `домен/?robots=1` (вместо 1 может быть что угодно, что даст true при !empty($qv['robots']) )
- по аналогии с robots открывается и карта сайта, например по адресу `домен/?sitemap=index`
- страница 404 не открывается в нужном шаблоне движка, потому что при отсутствии правил ЧПУ в `.htaccess` и обращении по адресу, к примеру `домен/my-page` сервер сразу начинает искать файл `домен/my-page/index.html` (или другое, в зависимости от настроек сервера), его конечно нет и сам сервер уже отдаёт 404 страницу, то есть до движка дело даже не доходит
