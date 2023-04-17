# VIP Local Development Environment

This environment is based on Docker; however, only minimal knowledge of Docker is actually required.

Please see [WPVIP - Create a VIP Local Development Environment](https://docs.wpvip.com/how-tos/local-development/use-the-vip-local-development-environment/) for WPVIP's official documentation.

This tool is under constant development by WPVIP, so refer to previous link for the most up-to-date info. That being said, WPVIP's documentation is not always clear, so the below is a walkthrough example to be used in conjunction with the official documentation.

## Create a basic local environment

1. Check all prerequisites required in the WPVIP documentation, i.e. VIP-CLI, Docker Desktop, etc.

2. Clone the site's Github repo to your local computer. Ideally to your desktop or somewhere that is easy to access.

> **Note**
> You may need to configure your GitHub account with an SSH key, since our repos are private and live under the WPVIP parent account.
>

3. Via command line, run `vip @xxxx.production dev-env create --slug={$site_slug}` to create your local dev environment.

    - This launches the WPVIP local dev env setup wizard.
    - `@{$vip_app_id}.production` tells the tool we want to create a local environment that matches the **production** version of our site (which is VIP app ID **vip_app_id**)
      - Running `vip app list` will give a list of all of our sites along with their VIP app ID #s
    - `--slug={$site_slug}` tells the tool to give a certain name to your local environment. **You can name this anything you want**, but it's generally best to have it match with whatever we call it on WPVIP.
    - `Multisite` prompt: match the live setup. Usually true.
    - `PHP` version: match live setup.
    - `WordPress` version: match live setup.
    - Redirect to live site for media files -- true is recommended as it means you won't have to pull images locally
    - Source `vip-go-mu-plugins -- image/demo` (this is code that WPVIP runs on the back-end of all sites)
    - Source `application code -- local/custom;` paste the full folder path to the repo you cloned in Step 2
    - `Enterprise Search -- false` (I don't think we use this on any of our sites)
    - `phpMyAdmin -- true` is recommended but optional if you prefer to interact with the database some other way
    - `XDebug -- false` - @todo investigate incorporating this into our workflow (good way to debug PHP)
    - `MailHog -- false`

4. Run `vip dev-env start --slug {$site_slug}` to launch the local environment you just created.
    - This may take few minutes to run, as it is spinning up a Docker container, pulling down WordPress, pulling down the `vip-mu-plugins`, and setting up the environment.
    - In the future you can run the command with the `-S` flag to skip rebuilding, for a somewhat faster load time.

5. Import the database locally. [Please see the full WPVIP tutorial on this here](https://docs.wpvip.com/how-tos/dev-env-add-content/).
    - Export the most recent database backup from WPVIP dashboard.
    - Unzip the downloaded `.gzip` file.
      - If using WPVIP Dashboard Database Export, export is a single `.sql` file that will have a timestamped name.
    - To make things easier in the CLI, rename to **xxx-xxx.sql**. For the import process, WPVIP says the file must be saved to the home directory of the current user (Windows: C:\Users\current-user\xxx-xxx-xxx.sql, Mac: /Users/current-user/xxx-xxx-xxx.sql)
    - Open the `.sql` file and **delete** the following:
      - Everything related to the `wp_users` and `wp_usermeta` tables. We do this because WPVIP's dev-env tool creates a special "vipgo" user account for our local dev environment, and we don't want to overwrite that.
    - Run `vip dev-env --slug={$site_slug} import sql xxxxxx-sql.sql`
    - Once this is imported, there are still a handful of database changes we'll need to make.
      - `wp_options` table: both `siteurl` and `home` should be your local domain URL, something like `http://{$site_slug}.vipdev.site` (can do https if you have that configured for local)
      - `wp_site` table: `domain` should be your local domain URL (do not include `http://`)
      - `wp_sitemeta` table: `site_name` should be your local domain URL (do not include `http://`)
      - `wp_blogs` table: Ensure all `domain` values have the proper `{$site_slug}.vipdev.site` suffixes
      - Repeat this for all wp_[#]_options tables
      - After updating, restart the vip dev-env environment

6. Visit your local URL admin: `{$site_slug}.vipdev.site/wp-admin` (or whatever URL what generated when you ran step #4 above)
    - Login username `vipgo` password `password`

7. Run `vip dev-env --slug={$site_slug} exec -- wp super-admin add vipgo`
    - `vipgo` user is automatically a super-admin on single-site installs, but for multisite you need to manually add.
   - To verify this was successful, reload the `/wp-admin` page. You should be able to see 'Plugins' on the menu, and the top admin bar under 'My Sites' should have a 'Network Admin' option.

8. Check that all network sites are present
   - WP-Admin -> My Sites -> Network Admin -> Sites
   - For each site, ensure that you are able to:
     - 8.1. Visit the site. Make sure it loads **locally** and doesn't redirect to the live site. The styling will look completely off and that is okay, because we still need to build the theme assets (next step)
     - 8.2. Load the Dashboard for that individual site

9. Build theme assets
   - At this point, visiting your local site `http://{$site_slug}.vipdev.site/` will show the correct content, but the styling will probably be broken. That's because we still need to build our theme assets.
   - Navigate to `themes/nameofthetheme/` and run `npm install` and then `npm run gulp`
     - This will build our CSS & JS assets, which are in our `/themes/nameofthetheme/dist/` folder. This folder is excluded from git, as we use CircleCI for our build process on the live sites.
     - After the command completes, run `vip dev-env --slug={$site_slug} exec -- wp cache flush --all-tables` to flush the cache

10. At this point, your local site should be *nearly* identical to the live site.

- Certain images are going to be missing, and that's okay. During the `dev-env create` setup wizard, we had an option for `--media-redirect-domain`. This enables our local site to pull images from the live site, but it only works for images that are attached/loaded via wordpress (i.e. featured images, or anything loaded via `wp_get_attachment`, etc). Images that are hard-coded as HTML or manually set as background-image via CSS will usually not load locally. This could probably be fixed, but unless it is mission-critical for a specific dev task, it's probably not worth the effort. Note that you can still upload images to the local site via WP-Admin and they will be available to use as you would on the live site.

- To stop your dev environment: `vip dev-env --slug={$site_slug} stop`
- To restart your dev environment: `vip dev-env --slug={$site_slug} start`
- To view all your available local dev environments: `vip dev-env list`
- To view all vip dev env commands: `vip dev-env -h`

11. Additional steps/recommendations

- **Recommended**: Go to Network Admin -> Users and edit user for vipgo. Change your 'admin color scheme' to something other than default. That way it will always be obvious that you are currently on your local dev, rather than a live environment. Recommended to do this for preprod and develop environments too, so you never run the risk of accidentally editing something on the live site.

**Additional Notes for WPVIP Local Dev Environment:**

- Wordpress Debug Log: can be found at `C:\Users\\[user]\\.local\share\vip\dev-environment\xxxx\log` (windows specific)
- `wp-config.php` can be found at: `C:\Users\\[user]\\.local\share\vip\dev-environment\xxxxx\config` (windows specific)
  - Certain sites/plugins might require certain 'development constants' to be set here, i.e. `define( 'JETPACK_DEV_DEBUG', true );`

---
> Last Modified: {docsify-updated}
