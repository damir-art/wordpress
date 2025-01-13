# the_date()
Функция `the_date()` в WordPress используется для вывода даты публикации текущего поста или страницы. В отличие от `the_time()`, она выводит дату только один раз для группы постов, опубликованных в один день. Это полезно, когда вы хотите избежать повторяющихся дат в списке постов.

**Описание:** Выводит дату публикации текущего поста или страницы, если она отличается от предыдущей.

```php
<?php the_date(); ?>
```

По умолчанию, функция выводит дату в формате, установленном в настройках WordPress.

## Примеры использования

Простое использование в цикле:
```php
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
  <h2><?php the_title(); ?></h2>
  <?php the_date(); ?>
  <?php the_content(); ?>
<?php endwhile; endif; ?>
```
Этот пример демонстрирует базовый цикл WordPress, где сначала выводится заголовок поста, затем дата публикации, и, наконец, содержание поста.

### Использование с кастомным форматом даты
```php
<?php the_date('F j, Y'); ?>
```
В этом примере дата будет выведена в формате "ПолноеНазваниеМесяца День, Год".

### Использование с кастомным текстом
```php
<?php the_date('Опубликовано d.m.Y'); ?>
```
Здесь перед датой добавляется текст "Опубликовано".

### Вывод только года
```php
<?php the_date('Y'); ?>
```
Этот пример выводит только год публикации.

### Проверка наличия даты
```php
<?php if ( get_the_date() != '' ) : ?>
  Дата публикации: <?php the_date('j M Y'); ?>
<?php else : ?>
  Дата публикации недоступна.
<?php endif; ?>
```
Этот пример проверяет, установлена ли дата публикации, и выводит соответствующую информацию.

### Вывод даты с кастомной разметкой
```php
<?php
$date = get_the_date('U');
echo '<time datetime="' . date('c', $date) . '">' . get_the_date() . '</time>';
?>
```
Этот пример выводит дату в формате HTML5 `<time>`, что улучшает SEO и доступность страницы.

### Вывод даты и времени в разных форматах
```php
<?php
$date_format = 'F j, Y';
$time_format = 'g:i a';
?>
Опубликовано: <?php the_date($date_format); ?> в <?php the_time($time_format); ?>
```
Этот пример выводит дату и время в двух разных форматах, разделенных словом "в".

### Вывод даты только для уникальных значений
```php
<?php the_date('F j, Y', '<h3>', '</h3>'); ?>
```
Этот пример выводит дату в формате заголовка `<h3>`, только если она отличается от предыдущей.

---

Эти примеры демонстрируют различные способы использования функции `the_date()` в разработке тем и плагинов для WordPress.