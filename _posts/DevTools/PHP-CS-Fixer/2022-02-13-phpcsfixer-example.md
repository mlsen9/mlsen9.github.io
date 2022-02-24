---
title: 通过 PHP-CS-Fixer 统一PHP代码风格教程
date: 2022-02-13 11:53:33 +0800
categories: [DevTools, CodeStyle]
tags: [php-devtools, php-cs-fixer]
---

此项目演示了 `PHPStorm` 如何配置 `PHP-CS-Fixer`, 并使用 `PHP-CS-Fixer` 修复项目中不规范的代码.

### 开发环境

- Platform: macOS Sierra 10.12.6
- PHPStorm Version: PhpStorm 2017.3.4
- PHP-CS-Fixer Version: PHP CS Fixer 2.10.0

### 步骤说明

1. 先安装 `PHP-CS-Fixer`, [详细安装说明](https://github.com/FriendsOfPHP/PHP-CS-Fixer/blob/2.10/README.rst).

2. 打开配置 `Preferences -> Tools -> External Tools` 点击左下角新增一条记录.

![External Tools](https://gitee.com/uploads/images/2018/0316/162608_5eb52ba9_599496.png "PHPStorm_Tools_ExternalTools.png")

3. 填写相关信息

![PHP-CS-Fixer-File](https://gitee.com/uploads/images/2018/0316/162954_eecf6026_599496.png "PHPStorm_PHP-CS-Fixer.png")

`.php_cs.dist 可以参考 example.php_cs.dist, 此文件我只是按照自己需要定制的一些规范. `


4. 如需要可为其配置快捷键, `Preferences -> Keymap -> External Tools`

![配置快捷键](https://gitee.com/uploads/images/2018/0316/163356_9c62255e_599496.png "PHPStorm_PHP-CS-Fixer_Keymap.png")

5. 代码演示

比如此条: `'array_syntax' => ['syntax' => 'short']`, 数组语法要求使用 `[]` 代替 `array()`.

```php

// 未修复之前
$array = array('a' => 'A', 'b' => 'B');
// 修复之后
$array = ['a' => 'A', 'b' => 'B'];

```



### php_cs.dist 代码示例

```php
<?php
/**
 * PHP 编码标准修复程序-配置文件
 *
 * @link 项目主页 https://github.com/FriendsOfPHP/PHP-CS-Fixer
 * @link 配置示例 https://github.com/FriendsOfPHP/PHP-CS-Fixer/blob/2.10/.php_cs.dist
 */


$config = PhpCsFixer\Config::create()
    ->setRiskyAllowed(true)
    ->setRules([
        // 数组语法, 推荐使用 short 语法.
        // short => "[]"; long => "array()"
        'array_syntax' => ['syntax' => 'short'],
        // list 语句语法, 推荐使用 long 语法.
        // 例：$array = [1, 2, 3];
        // short => "[$a, $b, $c] = $array;"; long => "list($a, $b, $c) = $array;"
        'list_syntax' => ['syntax' => 'long'],
        // 命名空间声明之后必须有一个空行
        'blank_line_after_namespace' => true,
        // 确保PHP打开标记没有在同一行上的代码，并且后面跟着一个空行.
        'blank_line_after_opening_tag' => true,
        // 使用 __DIR__ 代替 dirname(__FILE__)
        'dir_constant' => true,
        // 使用 elseif 代替 else if
        'elseif' => true,
        // 使用 preg 系列函数 代替 ereg 系列函数
        'ereg_to_preg' => true,
        // PHP代码必须使用 <?php 或 <?=
        'full_opening_tag' => true,
        // PHP 文件最后一行应移除结束标签
        'no_closing_tag' => true,
        // include/require 函数和文件路径中间用一个空格分开，文件路径不应该放在括号内.
        // include 'a.php'; 代替 include('a.php');
        'include' => true,
        // true、false, null 必须使用小写.
        'lowercase_constants' => true,
        // PHP 系统关键字必须使用小写.
        // 例：echo, break
        'lowercase_keywords' => true,
        // 类每个方法中间必须用一个空行分隔
        'method_separation' => true,
        // 使用 new 创建的类实例, 后面必须跟上大括号 "()". new A(); 代替 new A;
        'new_with_braces' => true,
        // 将PHP4风格的构造函数转换为 __construct
        'no_php4_constructor' => true,
        // PHP单行数组不应该有尾随逗号。
        'no_trailing_comma_in_singleline_array' => true,
        // 未使用的 use 必须删除
        'no_unused_imports' => true,
        // 在数组声明中, 每个逗号前都不能有空格.
        'no_whitespace_before_comma_in_array' => true,
        // 类中的 常量、成员变量、构造函数 等等之间的排序
        // 默认排序：['use_trait', 'constant_public', 'constant_protected', 'constant_private', 'property_public', 'property_protected', 'property_private', 'construct', 'destruct', 'magic', 'phpunit', 'method_public', 'method_protected', 'method_private']
        'ordered_class_elements' => true,
        // 类中导入的文件排序
        // sortAlgorithm  alpha => 按 A~Z 排序, length => 按长度排序.
        'ordered_imports' => ['sortAlgorithm' => 'length'],
        // PHP多行数组应该有一个尾随逗号.
        'trailing_comma_in_multiline_array' => true,
        // 在数组声明中，每个逗号后都必须有一个空格。
        'whitespace_after_comma_in_array' => true,
    ])
    ->setFinder(
        PhpCsFixer\Finder::create()
            ->exclude('tests/Fixtures')
            ->in(__DIR__)
    );

// special handling of fabbot.io service if it's using too old PHP CS Fixer version
try {
    PhpCsFixer\FixerFactory::create()
        ->registerBuiltInFixers()
        ->registerCustomFixers($config->getCustomFixers())
        ->useRuleSet(new PhpCsFixer\RuleSet($config->getRules()));
} catch (PhpCsFixer\ConfigurationException\InvalidConfigurationException $e) {
    $config->setRules([]);
} catch (UnexpectedValueException $e) {
    $config->setRules([]);
} catch (InvalidArgumentException $e) {
    $config->setRules([]);
}

return $config;
```





### 参考文献

> - [项目主页](https://github.com/FriendsOfPHP/PHP-CS-Fixer){:target="_blank"}
> - [官方配置示例](https://github.com/FriendsOfPHP/PHP-CS-Fixer/blob/2.10/.php_cs.dist){:target="_blank"}
