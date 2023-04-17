# Troubleshooting

## Fix `ERR_TOO_MANY_REDIRECTS`

![Screenshot of Google Chrome browser displaying an error](./_images/../../_images/ERR_TOO_MANY_REDIRECTS-error-in-chrome.png ':size=40%')

1. Clear browser data (suggested time range: 24 hours).
2. Delete Cookies on that specific site where this issue is happening.
3. Flush DB cache:
   - `vagrant ssh`
   - Navigate to the very top level of your VM: `cd ../..`
   - Access the site where you are having the issue: `cd srv/www/{$site_slug}`
   - `wp cache flush`
4. Check HTTP to HTTPS Redirects on the following tables:
   - wp_options - `siteurl` & `home` (http should be included)
   - wp_site - domain (url without http)
   - wp_sitemeta - `siteurl` (http should be included)
5. Confirm local domain are correct on `wp_blogs` table.

Helpful links:

- [How to Fix ERR_TOO_MANY_REDIRECTS on Your WordPress Site](https://kinsta.com/blog/err_too_many_redirects/)
- [How to Fix err_too_many_redirects in WordPress](https://www.hostinger.com/tutorials/how-to-fix-err-too-many-redirects#1-Deleting-Browser-Data)

## Fix redirect to the production environment

An issue you might run into when importing a site from production (or any other environment) to your local, is the your local URL redirecting to the source environment URL. To fix this try the following in Google Chrome:

- Open a new tab (_not_ in incognito mode).
- Open the dev console before doing going to any URL.
- Under the **Network** tab, make sure the "disable cache" option is checked.
- Go to your local environment's admin URL `{local-url.loc/wp-admin/}`.
- If this does not redirect, log in.
- Navigate to the Settings -> Permalinks settings page.
- Save/update settings without actually changing anything.

Although the root folder has composer.json, don't run composer install. Only for when updating specific plugins or installing new plugins.

----------------------------------------------------------------------

> Last Modified: {docsify-updated}
