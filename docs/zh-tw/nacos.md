# Nacos

一個 `Nacos` 的 `PHP` 協程客戶端，與 `Hyperf` 的配置中心、微服務治理完美結合。

## 安裝

```shell
composer require hyperf/nacos
```

### 釋出配置檔案

```shell
php bin/hyperf.php vendor:publish hyperf/nacos
```

```php
<?php

declare(strict_types=1);

return [
    // nacos server url like https://nacos.hyperf.io, Priority is higher than host:port
    // 'url' => '',
    // The nacos host info
    'host' => '127.0.0.1',
    'port' => 8848,
    // The nacos account info
    'username' => null,
    'password' => null,
    'guzzle' => [
        'config' => null,
    ],
    // 需要使用 hyperf/service-governance-nacos 组件提供之前的服务注册功能
//    'service' => [
//        // nacos server url like https://nacos.hyperf.io, Priority is higher than host:port
//        // 'url' => '',
//        // The nacos host info
//        'host' => '127.0.0.1',
//        'port' => 8848,
//        'service_name' => 'api',
//        // The nacos account info
//        'username' => null,
//        'password' => null,
//        'guzzle' => [
//            'config' => null,
//        ],
//        'instance' => [
//            'heartbeat' => 5
//        ],
//    ],
];

```

## 服務與例項

當前元件仍然保留了之前提供的服務註冊功能。

只需要安裝 `hyperf/service-governance-nacos` 元件，然後配置以下監聽器和自定義程序即可。

`Hyperf\ServiceGovernanceNacos\Listener\MainWorkerStartListener`
`Hyperf\ServiceGovernanceNacos\Listener\OnShutdownListener`
`Hyperf\ServiceGovernanceNacos\Process\InstanceBeatProcess`

然後增加如下配置，以監聽 `Shutdown` 事件

- config/autoload/server.php

```php
<?php
use Hyperf\Server\Event;
return [
    // ...other
    'callbacks' => [
        // ...other
        Event::ON_SHUTDOWN => [Hyperf\Framework\Bootstrap\ShutdownCallback::class, 'onShutdown']
    ]
];
```

