# wordpress-bash-ops
A collection of bash scripts and tools to perform ops on Wordpress

## Installation
All files are placed relatively to the root of any Linux file system.
So the files under etc need to be copied to /etc, the ones under bin need to be installed under /bin

After placing these files there you need to make them executable:
chmod 755 /bin/wp*

## Configuration
Almost all configuration can be performed by providing your personal settings in /etc/wp_ops.cfg
```
# Location on disk where the downloads are kept
wp_ops_main_dir="/var/lib/wordpress"

# Subdirectory for the core files
wp_ops_core_dir="core"

# Subdirectory for the plugins
wp_ops_plugin_dir="plugin"

# Where to send logs to
wp_ops_log="/var/log/wp_control.log"

# Where are your document roots excluding the hostname
# So for instance I keep all my document roots under /www/data
# and then the document root for example.com would be under /www/data/example.com
docroot_dir="/var/www"

# In case you need to append the document root e.g. /www/data/example.com/html. Then fill in "/html" here
docroot_append="/html"

# Where Wordpress keeps it's plugins. Do not change...
wpplugin_dir="wp-content/plugins"

# Composing a full path to the core and plugin locations
core_dir="$wp_ops_main_dir/$wp_ops_core_dir"
plugin_dir="$wp_ops_main_dir/$wp_ops_plugin_dir"
```

Apart from the configfile you also might want to change the MAILTO address in the cron file in /etc/cron.d/wp_ops.cron

## Usage
In principle keeping your files up to date is done automagically by the cron. Whenever it detects a new version it will retrieve this from Wordpress.org, extract it and then check if there are any vhosts with an older version than the new version. Both checks are performed on a daily basis.

Apart from this there are some executables you can use:

#### wpcore_fetch
```
wpcore_fetch [version]
```
This executable will fetch the latest Wordpress version from Wordpress.org and name it after the passed *version*. This executable is also used by the cron to fetch the latest.

#### wpcore_install
```
wpcore_install [vhost] [version]
```
This installs the passes *version* of Wordpress into the indicated *vhost*. You can also use this as an upgrade to a newer version.
After copying the files the script will also perform the database upgrade automatically by calling the script on the vhost. Make sure the vhost is resolvable from your webserver as well!

#### wpplugin_fetch
```
wpcore_fetch [plugin] [version]
```
This will fetch the latest version of the plugin from Wordpress.org and name it after the passed *version*. A helper script is needed in the plugin directory (e.g. /var/lib/wp_ops/plugins/[plugin]/fetch.sh) to fetch the specific plugin as not all plugin pages on Wordpress.org follow the same standard. (see also https://xkcd.com/927/)
Also keep in mind the plugin needs to have the exact same name as it is on disk in the wp-content/plugin directory when installed normally. Otherwise this script will not work!

#### wpplugin_install
```
wpcore_fetch [vhost] [plugin]
```
This installs the passes *plugin* into the indicated *vhost*. You can also use this as an upgrade to a newer version of the same plugin.
Also keep in mind the plugin needs to have the exact same name as it is on disk in the wp-content/plugin directory when installed normally. Otherwise this script will not work!

## Contributors

I created these scripts & tools to make my life easier as I was constantly upgrading the various WP installations I have. You are free to reuse or modify these scripts.

banpei-dbart
