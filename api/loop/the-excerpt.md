# the_excerpt()
Тег `the_excerpt()` в WordPress используется для вывода анонса или краткого описания записи (поста). Этот анонс может быть либо вручную введённым пользователем при создании записи, либо автоматически сгенерированным из начала содержимого записи. Давайте рассмотрим подробнее, как он работает и приведём несколько примеров его использования.

## Описание тега the_excerpt()
Когда вы создаёте запись в WordPress, у вас есть возможность добавить краткий анонс, заполнив специальное поле «Краткое описание» («Excerpt»). Если это поле заполнено, то функция `the_excerpt()` выведет этот текст. Если же оно пустое, WordPress автоматически создаст анонс, взяв первые 55 слов из основного текста записи и добавив многоточие (`...`).

## Примеры использования `the_excerpt()`
Простое использование на архивной странице:

```php
if ( have_posts() ) :
  while ( have_posts() ) : the_post();
    the_title('<h2>', '</h2>');
    the_excerpt();
  endwhile;
endif;
```

Добавление ссылки «Читать далее» (сработает если появится ..., т.е в анонсе должно быть 55 слов):

```php
function custom_excerpt_more( $more ) {
  return '... <a href="' . get_permalink() . '">Читать далее</a>';
}
add_filter('excerpt_more', 'custom_excerpt_more');

the_excerpt();
```

Изменение количества слов в автоматическом анонсе:

```php
function custom_excerpt_length( $length ) {
  return 20; // Количество слов
}
add_filter('excerpt_length', 'custom_excerpt_length');

the_excerpt();
```

Удаление многоточия в конце анонса:

```php
function remove_dots_from_excerpt($more) {
  return '';
}
add_filter('excerpt_more', 'remove_dots_from_excerpt');

the_excerpt();
```

Использование пользовательского HTML-тега для анонса:

```php
function custom_excerpt_output($output) {
  return '<p class="custom-excerpt">' . $output . '</p>';
}
add_filter('the_excerpt', 'custom_excerpt_output');

the_excerpt();
```

Отображение анонса только для определённых типов записей:

```php
if ( is_singular('post') || is_home() ) {
  the_excerpt();
}
```

Обработка анонсов для разных типов записей:

```php
if ( has_excerpt() && !is_page() ) {
  the_excerpt();
} else {
  the_content();
}
```

Создание кастомного анонса для определённого типа записей:

```php
function custom_post_type_excerpt( $post_id ) {
  if ( get_post_type( $post_id ) == 'product' ) {
    $excerpt = get_field( 'product_description' ); // ACF field
    echo wp_trim_words( $excerpt, 30 );
  } else {
    the_excerpt();
  }
}

custom_post_type_excerpt(get_the_ID());
```

Проверка наличия анонса перед выводом:

```php
if ( has_excerpt() ) {
  the_excerpt();
} else {
  echo 'У данной записи отсутствует анонс.';
}
```

Настройка автоматической генерации анонса с учётом HTML-разметки:

```php
function custom_excerpt_with_tags($text) {
  $raw_excerpt = $text;
  if ( '' == $text ) {
    $text = get_the_content('');
    $text = strip_shortcodes($text);
    $text = apply_filters('the_content', $text);
    $text = str_replace(']]>', ']]&gt;', $text);
    $excerpt_length = apply_filters('excerpt_length', 25);
    $excerpt_more = apply_filters('excerpt_more', '...');
    $words = preg_split('/(<[^>]+>)|(\s+)/', $text, $excerpt_length + 1, PREG_SPLIT_DELIM_CAPTURE | PREG_SPLIT_NO_EMPTY);
    if ( count($words) > $excerpt_length ) {
      array_pop($words);
      $text = implode('', $words) . $excerpt_more;
    } else {
      $text = implode('', $words);
    }
  }
  return $text;
}
add_filter('wp_trim_excerpt', 'custom_excerpt_with_tags');

the_excerpt();
```

Эти примеры показывают различные способы настройки и использования анонсов в WordPress. Вы можете адаптировать их под свои нужды, создавая уникальные решения для своих проектов.

## Разбор последнего примера

Последний пример демонстрирует настройку автоматической генерации анонса с сохранением HTML-разметки. По умолчанию WordPress обрезает текст до нужного количества слов и добавляет многоточие, но при этом удаляет все HTML-теги. Этот скрипт позволяет сохранить важные элементы разметки, такие как абзацы, списки и ссылки, делая анонс более читаемым и удобным для восприятия.

Давайте разберём этот код шаг за шагом:

### Шаг 1: Объявляем функцию `custom_excerpt_with_tags`

```php
function custom_excerpt_with_tags( $text ) {
  ...
}
```

Эта функция принимает аргумент `$text`, который представляет собой исходный текст анонса. Мы будем обрабатывать этот текст, чтобы создать анонс с сохранёнными HTML-элементами.

### Шаг 2: Получаем контент записи

```php
$raw_excerpt = $text;
if ( '' == $text ) {
  $text = get_the_content('');
  $text = strip_shortcodes( $text );
  $text = apply_filters('the_content', $text);
  $text = str_replace(']]>', ']]&gt;', $text);
}
```

Сначала мы проверяем, передан ли нам пустой текст. Если да, то получаем полное содержимое записи с помощью `get_the_content('')`. Затем удаляем шорткоды с помощью `strip_shortcodes`, применяем стандартные фильтры WordPress для контента с помощью `apply_filters('the_content', $text)` и заменяем символы `]]>` на `]]&gt;`, чтобы избежать проблем с XML-парсингом.

### Шаг 3: Разбиваем текст на части

```php
$excerpt_length = apply_filters('excerpt_length', 25);
$excerpt_more = apply_filters('excerpt_more', '...');
$words = preg_split('/(<[^>]+>)|(\s+)/', $text, $excerpt_length + 1, PREG_Split_Delim_Capture | PREG_Split_No_Empty);
```

Здесь мы определяем длину анонса с помощью фильтра `excerpt_length` (по умолчанию 25 слов) и устанавливаем текст окончания анонса с помощью фильтра `excerpt_more` (по умолчанию многоточие `...`). Далее используем регулярное выражение `preg_split` для разделения текста на отдельные части, включая HTML-теги и пробелы. Таким образом, каждый элемент массива `$words` будет содержать либо слово, либо HTML-тег.

### Шаг 4: Формируем анонс

```php
if ( count($words) > $excerpt_length ) {
    array_pop($words);
    $text = implode('', $words) . $excerpt_more;
} else {
    $text = implode('', $words);
}
return $text;
```

Теперь проверяем, превышает ли количество элементов массива `$words` установленную длину анонса. Если да, то удаляем последний элемент с помощью `array_pop` и добавляем текст окончания анонса. Если нет, то просто объединяем все элементы массива обратно в строку. В итоге возвращаем сформированный анонс.

### Применение фильтра

```php
add_filter('wp_trim_excerpt', 'custom_excerpt_with_tags');
```

Этот фильтр применяется к стандартному механизму генерации анонса в WordPress, позволяя использовать нашу новую функцию вместо стандартной обработки.

### Итоговый результат

```php
the_excerpt();
```

После применения нашего фильтра вызов `the_excerpt()` теперь будет выводить анонс с сохранённой HTML-разметкой, что сделает его более аккуратным и удобным для чтения.

Таким образом, этот скрипт позволяет создавать анонсы, которые сохраняют структуру и форматирование оригинального текста, что особенно полезно, когда в тексте используются списки, ссылки или другие HTML-элементы.
