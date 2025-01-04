# the_content()
Функция `the_content()` в WordPress используется для вывода содержимого текущего поста или страницы. Это основной способ отображения текста поста в шаблоне темы. Функция автоматически добавляет теги абзацев `<p>` вокруг текста, разбивая его на отдельные параграфы. Как и `the_permalink()`, эта функция предназначена для непосредственного вывода данных, а не для возврата значения.

**Описание:** выводит содержимое текущего поста или страницы.

**Использование:**
```php
<?php the_content(); ?>
```

## Примеры использования

Простое использование в цикле:
```php
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
  <h2><?php the_title(); ?></h2>
  <?php the_content(); ?>
<?php endwhile; endif; ?>
```
Этот пример демонстрирует базовый цикл WordPress, где сначала выводится заголовок поста, а затем его содержание.

Вывод содержания с кнопкой «Продолжить чтение»:
```php
<?php the_content('<button class="read-more">Продолжить чтение</button>'); ?>
```
Добавляем кнопку «Продолжить чтение», которая будет вести на полную версию поста.

#### 3. Обработка контента перед выводом:
```php
<?php
$content = apply_filters('the_content', get_the_content());
echo $content;
?>
```
В этом примере мы применяем фильтры к содержимому поста перед его выводом. Фильтры могут изменять контент, добавлять стили или вставлять дополнительные элементы.

Вывод содержимого без тегов абзаца:
```php
<?php
$content = get_the_content();
$content = strip_tags($content);
echo $content;
?>
```
Удаляем все HTML-теги из содержимого поста, оставляя только текст.

Вставка рекламы после первого абзаца:
```php
<?php
$content = get_the_content();
$content = explode("</p>", $content);
array_splice($content, 1, 0, "<div class='ad'>Рекламный блок</div>");
$content = implode("</p>", $content);
echo $content;
?>
```
Разбиваем содержимое на массив абзацев, вставляем рекламный блок после первого абзаца и снова собираем текст.

Замена коротких строк на списки:
```php
<?php
$content = get_the_content();
$content = preg_replace('/\n\s*\n/', "</p>\n<p>", $content);
$content = str_replace("\n", "</li>\n<li>", $content);
$content = '<ul><li>' . $content . '</li></ul>';
echo $content;
?>
```
Преобразуем короткие строки в маркированные списки.

Вывод содержимого в модальном окне:
```html
<div id="modal-content">
  <?php the_content(); ?>
</div>
<script>
document.getElementById('open-modal').addEventListener('click', function() {
  document.getElementById('modal').style.display = 'block';
});
</script>
```
Открываем содержимое поста в модальном окне при нажатии на кнопку.

Преобразование ссылок в контенте:
```php
<?php
$content = get_the_content();
$content = preg_replace_callback('/<a[^>]+href="([^"]+)"[^>]*>(.*?)<\/a>/i', function($matches) {
  return '<a href="' . esc_url($matches[1]) . '" target="_blank" rel="noreferrer noopener">' . $matches[2] . '</a>';
}, $content);
echo $content;
?>
```
Автоматически добавляем атрибуты `target="_blank"` и `rel="noreferrer noopener"` ко всем ссылкам в контенте.

Вывод содержимого с определённым классом:
```html
<div class="post-content">
  <?php the_content(); ?>
</div>
```
Обертываем содержимое поста в div с классом `.post-content`.

Вывод сокращённого содержимого:
```php
<?php
$content = get_the_content();
$content = wp_trim_words($content, 50, '...');
echo $content;
?>
```
Укорачиваем содержимое до 50 слов и добавляем многоточие в конце.

---

Эти примеры демонстрируют гибкость использования функции `the_content()` в различных сценариях разработки тем и плагинов для WordPress.
