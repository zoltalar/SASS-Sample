# Sample SASS Code
SASS code from one of the recent BuddyPress projects. Theme's main styles are organized based on Bootstrap 3 breakpoints (mobile first):

* `theme.scss` contains default styles and media queries for non-standard Bootstrap 3 breakpoints (marked as exceptions),
* `theme-xs.scss` contains styles for devices larger than 480px,
* `theme-sm.scss` contains styles for devices larger than 768px,
* `theme-md.scss` contains styles for devices larger than 992px,
* `theme-lg.scss` contains styles for devices larger than 1200px.

Each `.scss` theme file is then compiled separately using SASS into `.css` file:

```
cd /var/www/html/site/wp-content/themes/my-theme/
sass assets/sass/theme.scss:assets/css/theme.css --style compressed
sass assets/sass/theme-xs.scss:assets/css/theme-xs.css --style compressed
sass assets/sass/theme-sm.scss:assets/css/theme-sm.css --style compressed
sass assets/sass/theme-md.scss:assets/css/theme-md.css --style compressed
sass assets/sass/theme-lg.scss:assets/css/theme-lg.css --style compressed
```

For non-production sites, you may skip the `--style compressed` flag.

Each breakpoint specific CSS file is then loaded using WordPress `wp_print_styles` hook:

```
function mt_print_styles()
{
    $uri = get_stylesheet_directory_uri();

    wp_register_style( 'theme', $uri . '/assets/css/theme.css', array() );
    wp_register_style( 'theme-xs', $uri . '/assets/css/theme-xs.css', array( 'theme' ), false, 'screen and (min-width: 480px)' );
    wp_register_style( 'theme-sm', $uri . '/assets/css/theme-sm.css', array( 'theme' ), false, 'screen and (min-width: 768px)' );
    wp_register_style( 'theme-md', $uri . '/assets/css/theme-md.css', array( 'theme' ), false, 'screen and (min-width: 992px)' );
    wp_register_style( 'theme-lg', $uri . '/assets/css/theme-lg.css', array( 'theme' ), false, 'screen and (min-width: 1200px)' );

    wp_enqueue_style( 'theme' );
    wp_enqueue_style( 'theme-xs' );
    wp_enqueue_style( 'theme-sm' );
    wp_enqueue_style( 'theme-md' );
    wp_enqueue_style( 'theme-lg' );
}
add_action( 'wp_print_styles', 'mt_print_styles' );
```

Things worth noticing:

* Each major page component (and it's breakpoint specific styles) has it's own `.scss` file (e.g. `theme/_post-sm.scss`).
* All selectors and CSS properties are alphabetically ordered.