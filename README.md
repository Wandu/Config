Wandu Config
===

[![Latest Stable Version](https://poser.pugx.org/wandu/config/v/stable.svg)](https://packagist.org/packages/wandu/config)
[![Latest Unstable Version](https://poser.pugx.org/wandu/config/v/unstable.svg)](https://packagist.org/packages/wandu/config)
[![Total Downloads](https://poser.pugx.org/wandu/config/downloads.svg)](https://packagist.org/packages/wandu/config)
[![License](https://poser.pugx.org/wandu/config/license.svg)](https://packagist.org/packages/wandu/config)

Simple Config Based On Dot Array.

## Installation

```bash
composer require wandu/config
```

## Usage

```php
$config = new \Wandu\Config\Config([
    'host' => 'wandu.github.io',
    'log' => [
        'handler' => 'monolog',
        'path' => 'log/wandu.log',
    ],
]);

static::assertSame('wandu.github.io', $config->get('host'));
static::assertSame('wandu.github.io', $config['host']);

static::assertSame([
    'handler' => 'monolog',
    'path' => 'log/wandu.log',
], $config->get('log'));
static::assertSame([
    'handler' => 'monolog',
    'path' => 'log/wandu.log',
], $config['log']);

static::assertSame('log/wandu.log', $config->get('log.path'));
static::assertSame('log/wandu.log', $config['log']['path']);
static::assertSame('log/wandu.log', $config['log.path']); // you can use dot syntax in array!
```

### Use Default Value

```php
$config = new \Wandu\Config\Config([
    'debug' => false,
    'cache_dir' => null,
]);

static::assertSame(false, $config['debug']);
static::assertSame(null, $config['cache_dir']);
static::assertSame(null, $config['unknown']);

static::assertSame(false, $config->get('debug', true));
static::assertSame(null, $config->get('cache_dir', "/")); // check by array_key_exists
static::assertSame("unknown text..", $config->get('unknown', "unknown text.."));
```

### Config Types

- PHP (example, [test.config.php](../../../tests/Config/test.config.php))
- Env(require `m1/env`) (example, [test.config.env](../../../tests/Config/test.config.env))
- YAML(require `symfony/yaml`) (example, [test.config.yml](../../../tests/Config/test.config.yml))

```php
$config = new \Wandu\Config\Config();

$config->append(new \Wandu\Config\Loader\PhpLoader(__DIR__ . '/test.config.php'));
$config->append(new \Wandu\Config\Loader\EnvLoader(__DIR__ . '/test.config.env'));
$config->append(new \Wandu\Config\Loader\YmlLoader(__DIR__ . '/test.config.yml'));

static::assertSame([
    'foo' => 'foo string',
    'vendor1' => [
        'service1' => [
            'name' => 'vendor1 service1 name..',
            'path' => 'vendor1 service1 path..',
        ],
        'service2' => [
            'name' => 'vendor1 service2 name..',
            'path' => 'vendor1 service2 path..',
        ],
    ],
    'vendor2' => [
        'service1' => [
            'name' => 'vendor2 service1 name..',
            'path' => 'vendor2 service1 path..',
        ],
        'service2' => [
            'name' => 'vendor2 service2 name..',
            'path' => 'vendor2 service2 path..',
        ],
    ],
    'env1' => 'what the',
    'env2' => false,
    'yml1' => [
        'yml11' => true,
    ],
    'yml2' => [
        'paths' => ['vendor/*', 'tests/*']
    ],
    'yml3' => [
        'yml3_1',
        'yml3_2',
    ],
], $config->toArray());
```
