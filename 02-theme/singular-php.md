# singular.php
Файл-шаблон одиночной страницы (не архивной):

    <?php get_header(); ?>
    <section class="section section--main">
      <div class="container">
        <div class="row">

          <?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
            <div class="col">
              <div class="bg-light p-3">
                <h1><?php the_title() ?></h1>
                <?php echo legioner_thumb(get_the_ID(), 'large'); ?>
                <?php the_content() ?>
                <div>
                  <span class="text-secondary"><?php the_time('j F Y'); ?></span>
                </div>
                <div class="mt-2">
                  <?php edit_post_link(); ?>
                </div>
              </div>
            </div> <!-- col -->
          <?php endwhile; else : ?>
            <p>Записей нет.</p>
          <?php endif; ?>

        </div> <!-- row -->
        <div class="row">
          <div class="col my-4">
            <?php the_posts_pagination(array('show_all' => true)) ?>
          </div>
        </div>
      </div> <!-- container -->
    </section>
    <?php get_footer(); ?>
