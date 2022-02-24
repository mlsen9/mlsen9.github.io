---
title: 通过 Composer 安装来自本地的软件包
date: 2022-02-13 11:53:33 +0800
categories: [DevTools, Composer]
tags: [composer-repo, package-manager]
---

### 目录结构示意

```
apps/
  my-app/
    test.php
    composer.json
packages/
  my-package1/
    src/
      TestClass.php
    composer.json
  ...
```

### composer.json 示例
```json
{
  "name": "peter/my-app",
  "description": "通过 Composer 安装来自本地的软件包",
  "type": "project",
  "minimum-stability": "dev",
  "repositories": [
    {
      "type": "path",
      "url": "../../packages/my-package1"
    },
    {
      "type": "path",
      "url": "../../packages/my-package2"
    }
  ],
  "require": {
    "my/package1": "*",
    "my/package2": "*"
  }
}
```

### `package1 和 package2` composer.json 示例

```json
{
  "name": "my/package1",
  "description": "本地软件包1",
  "type": "library",
  "minimum-stability": "dev",
  "autoload": {
    "psr-4": {
      "Package1\\": "src"
    }
  }
}
```

```php
// TestClass.php 代码示例
namespace Package1;

class TestClass
{
    public function saySomething()
    {
        echo '本地软件包1';
    }
}

// test.php 代码示例
require(__DIR__ . '/vendor/autoload.php');
(new \Package1\TestClass())->saySomething();
```
