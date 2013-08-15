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



## How it works

### In Composer

WordPress is loaded as a _package_ by referencing its source on GitHub, while WordPress Packagist is loaded as a _repository_, which allows us to require plugins from it like we require packages from Packagist.

When either the `update` or `install` commands are run, the `wp-config-redirect` script is triggered. This script takes the following code:
```bash
<?php require dirname(dirname(__DIR__)) . '/config/wp-config.php';
```
and places it in `vendor/wordpress/wp-config.php`. If you recall, step 3 of the setup was to place the WordPress config file in `config/wp-config.php`. By default, WordPress is only looking for the config file in its root directory and one level up. Since we don't want to change WordPress core, we're running a script to place the contents of our config file in a place where WordPress can find it.


### In the file system

A few symlinks exist in locations that WordPress expects to find its files. If you take a look in `public`, you'll see three symlinks:

```bash
wp-admin -> ../vendor/wordpress/wordpress/wp-admin
wp-includes -> ../vendor/wordpress/wordpress/wp-includes
wp-login.php -> ../vendor/wordpress/wordpress/wp-login.php
```

This allows the WordPress admin to function from its home in `vendor`.