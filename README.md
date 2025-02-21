Yii2 Queue Analytics Module (PHP 8+)
===========================

The module collects statistics about working of queues of an application, and provides web interface
for research. Also the module allows to stop and replay any jobs manually.

[![License](https://poser.pugx.org/andrey-zhukovskiy/yii2-queue-monitor/license)](https://github.com/andrey-zhukovskiy/yii2-aws-s3/blob/2.x/LICENSE) 
[![Latest Stable Version](https://poser.pugx.org/andrey-zhukovskiy/yii2-queue-monitor/v)](//packagist.org/packages/andrey-zhukovskiy/yii2-queue-monitor) 
[![Total Downloads](https://poser.pugx.org/andrey-zhukovskiy/yii2-queue-monitor/downloads)](//packagist.org/packages/andrey-zhukovskiy/yii2-queue-monitor) 
[![Latest Unstable Version](https://poser.pugx.org/andrey-zhukovskiy/yii2-queue-monitor/v/unstable)](//packagist.org/packages/andrey-zhukovskiy/yii2-queue-monitor)

Installation
------------

The preferred way to install the extension is through [composer](http://getcomposer.org/download/).
Add to the require section of your `composer.json` file:

```
"andrey-zhukovskiy/yii2-queue-monitor": "~0.2"
```

Usage
-----

To configure the statistics collector, you need to add monitor behavior for each queue component. 
Update common config file:

```php
return [
    'components' => [
        'queue' => [
            // ...
            'as jobMonitor' => \zhuravljov\yii\queue\monitor\JobMonitor::class,
            'as workerMonitor' => \zhuravljov\yii\queue\monitor\WorkerMonitor::class,
        ],
    ],
];
```

There are storage options that you can configure by common config file:

```php
return [
    'container' => [
        'singletons' => [
            \zhuravljov\yii\queue\monitor\Env::class => [
                'cache' => 'cache',
                'db' => 'db',
                'pushTableName'   => '{{%queue_push}}',
                'execTableName'   => '{{%queue_exec}}',
                'workerTableName' => '{{%queue_worker}}',
            ],
        ],
    ],
];
```

If you want use migrations of the extension, configure migration command in console config:

```php
'controllerMap' => [
    'migrate' => [
        'class' => \yii\console\controllers\MigrateController::class,
        'migrationNamespaces' => [
            //...
            'zhuravljov\yii\queue\monitor\migrations',
        ],
    ],
],
```

And apply migrations.


### Web

Finally, modify your web config file to turn on web interface:

```php
return [
    'bootstrap' => [
        'monitor',
    ],
    'modules' => [
        'monitor' => [
            'class' => \zhuravljov\yii\queue\monitor\Module::class,
        ],
    ],
];
```

It will be available by URL `http://yourhost.com/monitor`.


### Console

There is console garbage collector:

```php
'controllerMap' => [
    'monitor' => [
        'class' => \zhuravljov\yii\queue\monitor\console\GcController::class,
    ],
],
```

It can be executed as:

```sh
php yii monitor/clear-deprecated P1D
```

Where `P1D` is [interval spec] that specifies to delete all records one day older.

[interval spec]: https://www.php.net/manual/en/dateinterval.construct.php