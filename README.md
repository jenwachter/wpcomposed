# WordPress, Composed

Load WordPress core and plugins via [Composer](http://getcomposer.org/).



## Setup

1. Clone the wpcomposed repository.

1. Setup your server so that `public` is the web root.

1. Run `composer update` to install WordPress in the `vendor` directory.

1. Copy the sample `wp-config.php` file to `config/wp-config.php`, keeping the couple lines that are already in there:

    ```bash
    cat vendor/wordpress/wordpress/wp-config-sample.php >> config/wp-config.php
    ```

1. In `config/wp-config.php`, make a couple changes:
    1. Update the database settings.
    2. Define the following two constants, which will tell WordPress where to find plugins, themes, and uploads:

    ```php
    define('WP_CONTENT_DIR', dirname(__DIR__) . '/public/assets');
    define('WP_CONTENT_URL', 'http://' . $_SERVER['HTTP_HOST'] . '/assets');
    ```

1. Create a symlink to hook the WordPress install into the `public` directory:

    ```bash
    ln -s ../vendor/wordpress/wordpress/ public/wp
    ```

1. Go to `yoursite.com/wp/wp-admin/` and run through the WordPress install.

1. Login to WordPress and go to Settings > General. Update the "Side Address (URL)" to be just `yoursite.com` instead of `yoursite.com/wp`.

1. Install some plugins using [WordPress Packagist](http://wpackagist.org/). Advanced Custom Fields, for example:

    ```json
    {
      "require": {
        "php": ">=5.2.4",
        "wordpress/wordpress": "4.1.1",
        "wpackagist/advanced-custom-fields": "4.2.*"
      }
    }
    ```



## How it works

WordPress is loaded as a _package_ by referencing its source on GitHub, while [WordPress Packagist](http://wpackagist.org/) is loaded as a _repository_, which allows us to require plugins from it like we require packages from Packagist.

When either the `update` or `install` Composer commands are run, the `wp-config-symlink` script is triggered, which creates a symlink from `vendor/wordpress/wp-config.php`, where WordPress expects to find a config file, to `config/wp-config.php`, where we manage our config file. Additionally, the `wp-htaccess-remove` scripted is triggered, which removes the .httaccess file created by WordPress, forcing it to use ours (`public/.htaccess`).
