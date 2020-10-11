# get_template_part() - многоразовый код
Если какой нибудь код в теме WordPress используется более одного раза, то его нужно вынести в отдельный файл и подключать с помощью функции get_template_part()

<p>
    Подключаемые <b>части шаблонов</b>, используются для того, чтобы рзместить повторяющийся код находящийся в разных шаблонах WordPress (index.php, single.php, page.php и т.д.), d отдельном файле. Это необходимо для удобного редактирования кода, для нескольких шаблонов сразу.
<p>
    Подключаемые части шаблонов, относятся к понятию программирования <b>DRY</b> <i>(don't repeat yourself)</i> - не повторяйся. В вашем проекте, не должно быть ни одного кода который бы повторился хотябы один раз.
</p>
<p>
    Чтобы подключить, ту или иную часть кода к теме, можно вместо require() или include() использовать более продвинутую функцию WordPress:</p>
<pre>
    get_template_part();
</pre>
<p>
    С помощью данной функции, можно подключать файлы содержащие код:</p>
<pre>
- хлебных крошек
- меню
- пагинация
- цикла и т.д.
</pre>

<h3>Примеры</h3>
<pre>
<b>// подключаем файл breadcrumbs.php</b>
get_template_part('breadcrumbs');

<b>// подключаем файл breadcrumbs-footer.php</b>
get_template_part('breadcrumbs', 'footer');
</pre>

<h2>Реальный пример</h2>
<pre>
<b>// пропишем в шаблоне page.php</b>
&lt;?php get_template_part('parts/content', 'page'); ?>

Данная строка, будет ссылаться на файл parts/content-page.php
</pre>

<p>
    Преимущества по сравнению с require() или include(), не нужно указывать путь до темы:</p>

<pre>
&lt;?php get_header(); ?&gt; // данная функция эквивалентна
&lt;?php include (TEMPLATEPATH . '/header.php'); ?&gt;</pre>

<h2>Пример шаблона content.php</h2>
<p>
    В файле content.php хранится код, который взят из цикла (loop).</p>
<p>
    Вставляется content.php в index.php, через функцию get_template_part() следующим образом:</p>
<pre>
&lt;?php while ( have_posts() ) : the_post(); ?&gt;
    &lt;?php get_template_part('content') ?&gt;
&lt;?php endwhile; ?&gt;
</pre>
<p>
    Тоже сделайте и в файле archive.php, чтобы можно было менять цикл лишь в одном месте.</p>

## Разное
- https://wp-kama.ru/function/get_template_part