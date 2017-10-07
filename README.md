# nginx-prestashop
[nginx](https://nginx.org/) config to run [prestashop](https://www.prestashop.com/) 1.6.x

this port is initially based on original settings & restrictions from [prestashop bundle](https://www.prestashop.com/en/download) aimed at [apache httpd](https://httpd.apache.org/)

v2 has been completely revised & improved &mdash; the ruleset has been rearranged in native [nginx](https://nginx.org/) way to utilize its power, thus forcing pages to load up to 5-10x faster<sup id="a1">[1](#f1)</sup>

<strong>[nb]</strong> the last rule at [fpm-prestashop.conf](fpm-prestashop.conf) assumes the ``upstream`` block w/ name ``php`` is declared at ``http`` section of your ``nginx.conf``

if not, you gotta declare it this way to get the whole thing working properly

```nginx

        upstream php {
            
            server unix:/var/run/php-fpm/php-fpm.sock;
        
        }

```

in turn, the above declaration assumes the ``php-fpm`` daemon is available thru socket ``unix:/var/run/php-fpm/php-fpm.sock``

if not, you gotta either configure your ``php-fpm`` daemon pool to listen this socket or replace ``unix:/var/run/php-fpm/php-fpm.sock`` w/ your pool settings respectively

for example, if your ``php-fpm`` daemon is running w/ default pool settings like ``listen = 127.0.0.1:9000`` replace ``unix:/var/run/php-fpm/php-fpm.sock`` w/ ``127.0.0.1:9000``

one more & last thing on ``php-fpm`` daemon pool config &mdash; it's highly recommended for security purposes either to leave the line ``security.limit_extensions`` commented or to uncomment it & set (cut) the value exactly to ``.php`` removing the rest 

```php

;security.limit_extensions = .php .php3 .php4 .php5

security.limit_extensions = .php

```

<strong id="f1">[1]</strong> the lion's share of the boost is achieving due to replacement of the snail ``rewrite`` rules w/ the rapid ``return`` / ``try_files`` ones[↺](#a1)

**glory to Ukraine!** 🇺🇦

Juliy V. Chirkov,  
[twitter.com/juliychirkov](https://twitter.com/juliychirkov)
