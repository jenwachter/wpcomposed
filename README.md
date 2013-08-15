# WordPress, Composed

Load WordPress core and plugins via [Composer](http://getcomposer.org/).



## Setup

1. Setup your server so that `public` is the web root.

1. Run `composer update` to install WordPress.

1. Copy `vendor/wordpress/wordpress/wp-config-sample.php` to `config/wp-config.php`. In this file, do a couple things:
    1. Update database settings
    2. Define the following two constants, which will tell WordPress where to find plugins, themes, and uploads:

    ```php
    define('WP_CONTENT_DIR', dirname(__DIR__) . '/public/assets');
	define('WP_CONTENT_URL', 'http://' . $_SERVER['HTTP_HOST'] . '/assets');
	```

1. Go to `yoursite/wp-admin/` and run through the WordPress install.

1. Install some plugins using [WordPress Packagist](http://wpackagist.org/). Advanced Custom Fields, for example:

```javascript
"require": {
	"php": ">=5.2.4",
	"wordpress/wordpress": "3.6"
    "wpackagist/advanced-custom-fields": "4.2.*"
}
```



## How it works

WordPress is loaded as a _package_ by referencing its source on GitHub, while [WordPress Packagist](http://wpackagist.org/) is loaded as a _repository_, which allows us to require plugins from it like we require packages from Packagist.

When either the `update` or `install` Composer commands are run, the `wp-config-symlink` script is triggered, which creates a symlink from `vendor/wordpress/wp-config.php`, where WordPress expects to find a config file, to `config/wp-config.php`, where we manage our config file.

In addition to the symlink created by our Composer-hooked script, a few more symlinks exist in locations that WordPress expects to find its files. If you take a look in `public`, you'll see three symlinks:

```bash
wp-admin -> ../vendor/wordpress/wordpress/wp-admin
wp-includes -> ../vendor/wordpress/wordpress/wp-includes
wp-login.php -> ../vendor/wordpress/wordpress/wp-login.php
```

This allows the WordPress admin to function from its home in `vendor`.
