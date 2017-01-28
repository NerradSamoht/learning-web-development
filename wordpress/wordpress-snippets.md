# WordPress Snippets

Remove query strings \(?\) from static resources:

```php
function project_remove_query_string_script( $src ) {

    $parts = explode( '?ver', $src );

    return $parts[0];

}

add_filter( 'script_loader_src', 'project_remove_query_string_script', 15, 1 );

add_filter( 'style_loader_src', 'project_remove_query_string_script', 15, 1 );
```

Dequeue unnecessary scripts:

```php
function project_dequeue_scripts() {

    wp_dequeue_script( 'handle' );

    wp_deregister_script( 'handle' );

}

add_action( 'wp_print_scripts', 'project_dequeue_scripts' );
```

Dequeue unnecessary styles:

```php
function project_dequeue_styles() {

    wp_dequeue_style( 'handle' );

    wp_deregister_style( 'handle' );

}

add_action( 'wp_enqueue_scripts', 'project_dequeue_styles', 20 );
```

Defer parsing of JavaScript: \(source - http://www.onlinemediamasters.com/why-is-wordpress-so-slow\)

```php
if (!(is_admin() )) {

    function defer_parsing_of_js ( $url ) {

        if ( FALSE === strpos( $url, '.js' ) ) return $url;

        if ( strpos( $url, 'jquery.js' ) ) return $url;

        // return "$url' defer ";

        return "$url' defer onload='";

    }

    add_filter( 'clean_url', 'defer_parsing_of_js', 11, 1 );

}
```



Load Contact Form 7 style and scripts based only if post type contains cf7 shortcode: \(source - https://dannyvankooten.com/only-load-contact-form-7-scripts-when-needed\)\)



function project\_cf7\_dequeue\_scripts\(\) {

    $load\_scripts = false;

    if\( is\_singular\(\) \) {

        $post = get\_post\(\);

        if\( has\_shortcode\($post-&gt;post\_content, 'contact-form-7'\) \) {

                $load\_scripts = true;

        }

    }

    if\( ! $load\_scripts \) {

        wp\_dequeue\_script\( 'contact-form-7' \);

        wp\_dequeue\_style\( 'contact-form-7' \);

    }

}

add\_action\( 'wp\_enqueue\_scripts', 'project\_cf7\_dequeue\_scripts', 99 \);

Echo scripts running on page: \(source: http://wordpress.stackexchange.com/questions/54064/how-do-i-get-the-handle-for-all-enqueued-scripts\)



function project\_inspect\_scripts\(\) {

    global $wp\_scripts;

    foreach\( $wp\_scripts-&gt;queue as $handle \) :

        echo $handle . ' \| ';

    endforeach;

}

add\_action\( 'wp\_print\_scripts', 'project\_inspect\_scripts' \);

