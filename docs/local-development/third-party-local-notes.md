# Set up a third-party local development app

Overall the process to follow to have everything set up is documented on [this link](https://docs.wpvip.com/how-tos/third-party-local-development/) but there are a few "extra" steps that to keep in mind:

## - [Step 1: Add the site’s codebase](https://docs.wpvip.com/how-tos/third-party-local-development/#h-step-1-add-the-site-s-codebase)

SSH connection with GitHub is mandatory to be able to clone a WPVIP remote repo. The steps to generate a new SSH key and adding it tothe ssh-agent can be found [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

## - [Step 2: Add vip-go-mu-plugins](https://docs.wpvip.com/how-tos/third-party-local-development/#h-step-2-add-vip-go-mu-plugins)

An SSH connection is required, but you should already be all set since you established the connection in the previous step.

Once you've cloned the repository, it's important to confirm that all the necessary dependencies are installed. To do this, start by running the following command:

```shell
git submodule update --init --recursive
composer install
npm install
```

> Mac users: `composer` can be installed using [Homebrew](https://docs.brew.sh/)

## [Step 3: Set up the object cache](https://docs.wpvip.com/how-tos/third-party-local-development/#h-step-3-set-up-the-object-cache)

At this point, if you have a problem with the `memcache` class, simply delete the `object-cache.php` file from the `wp-content` directory.

## [Step 4: Update _wp-config.php_](https://docs.wpvip.com/how-tos/third-party-local-development/#h-step-4-update-wp-config-php)

The following line should be added to the `wp-config.php` file: `define( 'VIP_GO_APP_ENVIRONMENT', 'local' );`

## [Step 5: Create an admin user via WP-CLI](https://docs.wpvip.com/how-tos/third-party-local-development/#h-step-5-create-an-admin-user-via-wp-cli)

> Multisite setups - your local admin user should have super-admin permissions

## [Step 6: Finishing up](https://docs.wpvip.com/how-tos/third-party-local-development/#h-step-6-finishing-up)

At this point, everything should be all ready to go! Confirm everything works.

----------------------------------------------------------------------

> Last Modified: {docsify-updated}
