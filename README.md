# Laravel Elastic Common Scheme (ECS) Logging 
This package is a fork of AviationCode\EcsLogging, it tries to fix some bugs.


## Installation

You can install the package via composer:

```bash
composer require ramibadrpro/laravel-ecs-logging
```

It's recommended to require `jenssegers/agent` which will add user agent logging support.

```bash
composer require jenssegers/agent
```

Optionally, you can publish the config file with:

```bash
php artisan vendor:publish --provider="AviationCode\EcsLogging\EcsLoggingServiceProvider" --tag="config"
```

Register log driver in `config/logging.php`

```php
return [
    'channels' => [
        // ... Other channels

        'ecs' => [
            'driver' => 'ecs',
            'path' => storage_path('logs/ecs/laravel.json'),
            'level' => 'debug',
            'days' => 14,
        ],
    ]
];
```

If you want to use this driver as the only logging method define `LOG_CHANNEL=ecs` in your `.env` or add the `ecs` channel into your stack driver.

All `Log::xxx()` calls get logged into json file. This file can be picked up by filebeat which sends it to your logstash or elasticsearch instance.

### Configure filebeat

Add the following to your `/etc/filebeat/filebeat.yml` file

```yaml
filebeat.inputs:
  - type: log
    enabled: true
    paths:
      - /path-to-your-laravel-app/storage/logs/ecs/*.json
    json:
      message_key: message
      keys_under_root: true
      overwrite_keys: true
```
## BUGS :
- Current bugs within the ecs-logs package : 

```Typed static property Aviation Code\Ecs Logging\Tracing\Correlate::$id must not be accessed before initialization```

### Fork Changelog :
13/05/2022 : 
Added the option to disable log file rotation through config file (Use the option `disable_rotate`)

## Credits

- [Ken Andries](https://github.com/DouglasDC3)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
