# Adding Integrity Attributes to Scripts

### Javascript files (external)
* Ensure your JS files are enqueued properly
* e.g: <br>
`wp_enqueue_script('slick-js', 'https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.6.0/slick.min.js', ['jquery'], null, true);`
* A CDN site like _https://cdnjs.com/libraries/slick-carousel_ has an SRI hash to copy with each cdn
* This code is then needed to add the hash to each JS file that has been enqueued: <br>
```
// Add integrity and crossorigin to specific scripts
add_filter('script_loader_tag', function ($tag, $handle, $src) {
    $scripts_with_integrity = [
        'slick-js' => 'sha512-An4a3FEMyR5BbO9CRQQqgsBscxjM7uNNmccUSESNVtWn53EWx5B9oO7RVnPvPG6EcYcYPp0Gv3i/QQ4KUzB5WA==',
        'plyr-js'  => 'sha512-A96+LbQyag3zmcXmcXECRxzgT4UoM2C1zIKNLv1oiZ3+AA5/WVGTAYlXfoRG3SYzEn4nU2P0v+HzGoQb4c6vIw==',
    ];

    if (isset($scripts_with_integrity[$handle])) {
        $integrity = esc_attr($scripts_with_integrity[$handle]);
        return '<script src="' . esc_url($src) . '" integrity="' . $integrity . '" crossorigin="anonymous" defer></script>';
    }

    return $tag;
}, 10, 3);
```

The same can be done with styles: <br>
`wp_enqueue_style('slick-style', 'https://cdnjs.cloudflare.com/ajax/libs/slick-carousel/1.6.0/slick.css', [], null);` <br>
Then adding the code to add the SRI hash: <br>
```
// Add integrity and crossorigin to specific styles
add_filter('style_loader_tag', function ($html, $handle, $href, $media) {
    $styles_with_integrity = [
        'slick-style' => 'sha512-WIWddQW7bHfs1gwICYIoXuifLb8gCPkE7Z/gq7QHk3pKuxjNs0E68Rn5c7Ig4cWguZW5CIvRroTj2GrSxsvUZQ==',
        'slick-theme' => 'sha512-6lLUdeQ5uheMFbWm3CP271l14RsX1xtx+J5x2yeIDkkiBpeVTNhTqijME7GgRKKi6hCqovwCoBTlRBEC20M8Mg==',
        'plyr'        => 'sha512-DiRh+DVNccYmgFhKZTU84F7KVYu5baUooO4zEY5rD2CLDp2hRZ/OvAFKbtpgHEgE99bPh6p17j0I/AN0qXDSCA==',
    ];

    if (isset($styles_with_integrity[$handle])) {
        $integrity = esc_attr($styles_with_integrity[$handle]);
        return "<link rel='stylesheet' id='{$handle}-css' href='{$href}' media='{$media}' integrity='{$integrity}' crossorigin='anonymous' />";
    }

    return $html;
}, 10, 4);

```
