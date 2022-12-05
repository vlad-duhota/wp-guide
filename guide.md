---
# echo field(acf)
---

```
<?php the_field("text"); ?>
```

---
# save field(acf)
---

```
<?php $text = get_field("text"); ?>         
```

---
# repeater(acf)
---

```
 <?php if( have_rows('repeater') ): ?>
    <ul>
        <?php while( have_rows('repeater') ): the_row(); 
            $image = get_sub_field('img');?>
            <li>
                <?php echo wp_get_attachment_image( $image, 'full' ); ?>
                <p><?php the_sub_field('text'); ?></p>
            </li>
        <?php endwhile; ?>
    </ul>
<?php endif; ?>        
```

---
# options page(acf)
---

```
<?php if( have_rows('nav', 'option') ): ?>
    <ul class="nav">
        <?php while( have_rows('nav', 'option') ): the_row();  ?>
            <li>
                <a href="<?php the_sub_field('link')?>"><?php the_sub_field('text')?></a>
            </li>
        <?php endwhile; ?>
    </ul>
<?php endif; ?>     
```

---
# blog archive(archive.php)
---

```
<section class="blog">
    <div class="container">
        <ul class="blog__list">
            <?php if ( have_posts() ) {
                while ( have_posts() ) {
                    the_post(); ?>
                        <li class="blog__item">
                            <a href="<?php the_permalink() ?>">
                                <img class="blog__item-img" src=" <?php echo get_the_post_thumbnail_url()?>">
                            </a>
                            <h4 class="blog__item-title"><?php the_title() ?></h4>
                            <p class="blog__item-date"><?php echo get_the_date('d/m/Y')?></p>
                            <p class="blog__item-content">   <?php echo wp_trim_words(get_the_content(), 28)?> </p>
                            <a href="<?php the_permalink() ?>" class="blog__item-more">Read more</a>
                        </li>                     
                <?php } 
            }?>
        </ul>
        <ul class="blog__paggination">
            <?php siteDefPaging() ?>
        </ul>
    </div>
</section>
```

---
# blog posts page(home.php)
---

```
<section class="blog">
    <div class="container">
        <ul class="blog__list">
            <?php $args = [ 'post_type' => 'post', ]; 
                $paged = ( get_query_var( 'paged' ) ) ? get_query_var( 'paged' ) : 1;
                    $query = new WP_Query( array(
                        'paged' => $paged,
                        'posts_per_page' => 4
                        ) );
                    ?>
                    <?php query_posts([
                        'post_type' => 'post', 
                        'paged' => $paged,
                        'posts_per_page' => 4
                        ]) 
                    ?>
            <?php if ( have_posts() ) :
                while ( have_posts() ) :
                    the_post(); ?>
                        <li class="blog__item">
                            <a href="<?php the_permalink() ?>">
                                <img class="blog__item-img" src=" <?php echo get_the_post_thumbnail_url()?>">
                            </a>
                            <a href="<?php the_permalink() ?>" class="blog__item-title">
                                <h4><?php the_title() ?></h4>
                            </a>
                            <p class="blog__item-date"><?php echo get_the_date('d/m/Y')?></p>
                            <p class="blog__item-content">   <?php echo wp_trim_words(get_the_content(), 28)?> </p>
                            <a href="<?php the_permalink() ?>" class="blog__item-more">Read more</a>
                        </li>                     
                <?php endwhile;
            endif;?>
        </ul>
        <ul class="blog__paggination">
             <?php siteDefPaging($query) ?>
        </ul>
    </div>
</section>
```

---
# post page(single.php)
---

```
<section class="post">
    <div class="container">
        <div class="sidebar__container">
            <img class="post__img" src=" <?php echo get_the_post_thumbnail_url()?>">
            <p class="post__info">
                <span class="post__info-date"><?php echo get_the_date('d/m/Y')?></span>|
                <span class="post__info-category">
                    <?php foreach(get_the_category() as $cat) : ?>
                    <?php echo $cat -> name?> | 
                    <?php endforeach; ?>
                </span>
            </p>
            <h2 class="post__name"><?php the_title() ?></h2>
            <div class="post__content">
                <?php the_content()?>
            </div>
        </div>
    </div>
</section>
```

---
# register custom post type(додати у functions.php)
---

```
add_action( 'init', 'register_post_types' );
function register_post_types() {
  register_post_type('project', [
    'labels' => [
      'name'               => 'Проекти', // основное название для типа записи
      'singular_name'      => 'Проект', // название для одной записи этого типа
      'add_new'            => 'Додати проект', // для добавления новой записи
      'add_new_item'       => 'Додавання проекту', // заголовка у вновь создаваемой записи в админ-панели.
      'edit_item'          => 'Редагування проекту', // для редактирования типа записи
      'new_item'           => 'Нова проект', // текст новой записи
      'view_item'          => 'Подивитися проект', // для просмотра записи этого типа.
      'search_items'       => 'Знайти проект', // для поиска по этим типам записи
      'not_found'          => 'Не знайдено', // если в результате поиска ничего не было найдено
      'not_found_in_trash' => 'Не знайдено у корзині', // если не было найдено в корзине
      'menu_name'          => 'Проекти', // название меню
    ],
    'menu_icon'          => 'dashicons-portfolio',
    'public'             => true,
    'menu_position'      => 5,
    'supports'           => ['title', 'thumbnail', 'page-attributes'],
    'has_archive'        => true,
    'rewrite'            => ['slug' => 'projects'],
    // 'numberposts'        => -1
   ] );

// Offers category
   register_taxonomy('project-categories', 'project', [
    'labels'        => [
      'name'                        => 'Категорії',
      'singular_name'               => 'Категорія проекту',
      'search_items'                => 'Шукати категорії',
      'popular_items'               => 'Популярні категорії',
      'all_items'                   => 'Усі категорії',
      'edit_item'                   => 'Редагувати категорію',
      'update_item'                 => 'Оновити категорію',
      'add_new_item'                => 'Додати нову категорію',
      'new_item_name'               => 'Нова назва категорії',
      'separate_items_with_commas'  => 'Відділити категорію комами',
      'add_or_remove_items'         => 'Додати або видалити категорію',
      'choose_from_most_used'       => 'Обрати найпопулярнішу категорію',
      'menu_name'                   => 'Категорії',
    ],
    'hierarchical'  => true,
    'show_in_nav_menus' => true,
    'show_in_menu' => true,
    'has_archive' => true,
  ]);
}
```

---
# search(search.php) 
---

```
 <main>
    <div class="container" style="padding-top: 40px; padding-bottom: 40px">
    <h2 class="page-title">Результати пошуку <?php echo get_search_query(); ?> :</h2>
        <?php if ( have_posts() ): ?>
            <ul class="search__list">
                <?php while( have_posts() ): ?>
                    <?php the_post(); ?>
                        <li class="card" style="max-width: 290px">
                            <h3 class="card__title">
                                <a href="#"><?php the_title(); ?></a>
                            </h3>
                            <time class="card__date" datetime="<?php echo get_the_time('d.m.Y')?>"><?php echo get_the_time('d.m.Y')?></time>
                            <p class="card__content">
                            <img src="<?php echo carbon_get_post_meta(get_the_ID(), 'project_mini_img')?>">
                             <?php echo carbon_get_post_meta(get_the_ID(), 'project_desc')?>
                            </p>
                            <a href="#" class="card__more">
                                Дізнатись більше
                                <img src="<?php echo get_template_directory_uri() ?>/assets/img/right-arrow.png">
                            </a>
                        </li>
                <?php endwhile; ?>
                <?php else: ?>
                <p style="text-align: center; margin-top: 20px">Результатів не знайдено</p>
                <a href="<?php echo get_site_url()?>" style="color: #000; display: block; text-align: center;text-decoration: underline; margin-top: 20px" class="href">Повернутись на головну сторінку</a>
            </ul>
        <?php endif; ?>
    </div>
</main>
```

---
# search form
---

```
<form action="<?php bloginfo( 'url' ); ?>" method="get" class="header__form">
    <input type="search" value="<?php if(!empty($_GET['s'])){echo $_GET['s'];}?>" name="s" class="header__form-input" placeholder="Пошук..." required autocomplete="off">
    <button type="submit" class="header__form-btn"><img src="<?php echo get_template_directory_uri() ?>/assets/img/loupe.svg"></button>
</form>
```