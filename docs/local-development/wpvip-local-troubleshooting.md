# Troubleshooting

See [VIP's troubleshooting section](https://docs.wpvip.com/technical-references/vip-local-development-environment/troubleshooting-dev-env/) first.

## Port conflicts

One issue they don't cover well is resolving port conflicts. If you spin up a local environment and its URLs include port numbers - for example, <http://xxxxx.vipdev.lndo.site/:8080> - that means VIP local was not able to use the default port 80. To find out what's using that port, you can use the command

`netstat -anv | grep 127.0.0.1.80`

This will give you a whole group of information about whatever is using port 80. Grab the third number after LISTEN, which is the process ID (PID), and then run

`ps -Ao user,pid,command | grep -v grep | grep PID`

(Just replace the all-caps PID at the end with the actual process ID, and leave the lowercase "pid" as-is.) This second command will give you the name of the process that is using port 80. From there, you'll need to find out where that software is installed or configured. If possible, uninstall the software. If that's not possible, configure it to use a different port number. If that's also not possible, the last resort is to manually edit your database settings to use whichever port VIP Local has assigned at the time. The tricky part is, each time you start the local environment, that port number changes, so you'll be frequently editing `siteurl` and `home` and any other applicable values for MultiSites.

## HTTPS links

If your browser throws an error due to links on the local site being HTTPS, and you don't have SSL set up on localhost, you can either:

- In the database `wp_options` table, make sure `option_name: siteurl` and `option_name: home` values are set to **http**://[url] instead of **https**://[url]
  - If you change this, need to navigate to WP Admin>Permalinks and click 'save changes' without actually changing anything. This will rewrite your permalinks to use HTTP.
- Or, [set up SSL on your localhost](https://docs.wpvip.com/how-tos/dev-env-advanced-usage/#h-add-a-trusted-ca-certificate).

----------------------------------------------------------------------

> Last Modified: {docsify-updated}
