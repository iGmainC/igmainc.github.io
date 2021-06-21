---
title: NextCloud 维护状态的解决方法
tags: Linux Docker NextCloud
---

# NextCloud 维护状态的解决方法

Docker 更新镜像后 NextCloud 容器一直处于维护状态，无法自动解除。

修改数据目录下的 `config/config.php`

```php
<?php
$CONFIG = array (
  'htaccess.RewriteBase' => '/',
  'memcache.local' => '\\OC\\Memcache\\APCu',
  'apps_paths' =>
  array (
    0 =>
    array (
      'path' => '/var/www/html/apps',
      'url' => '/apps',
      'writable' => false,
    ),
    1 =>
    array (
      'path' => '/var/www/html/custom_apps',
      'url' => '/custom_apps',
      'writable' => true,
    ),
  ),
  'instanceid' => 'oc7fxhmw0g8d',
  'passwordsalt' => 'ecGlcUnVricnR8qXL7T0D1d2/IVAz+',
  'secret' => 'h6tAuB8S7lxa8HHlK+Pg2t6utMK20ttPOprPHnPuEYXn+Wzf',
  'trusted_domains' =>
  array (
          0 => 'xxx',
  ),
  'datadirectory' => '/var/www/html/data',
  'dbtype' => 'mysql',
  'version' => '21.0.2.1',
  'overwrite.cli.url' => 'http://xxx:8000',
  'dbname' => 'nextcloud',
  'dbhost' => 'mysql:3306',
  'dbport' => '',
  'dbtableprefix' => 'oc_',
  'mysql.utf8mb4' => true,
  'dbuser' => 'oc_igmainc',
  'dbpassword' => 'yRibpmAoxLptk1On7lZFjye5cVOiYI',
  'installed' => true,
  'loglevel' => 0,
  'maintenance' => false,
  'theme' => '',
);
```

把 `'maintenance' => true` 改为 `false`，或者直接删掉这句。