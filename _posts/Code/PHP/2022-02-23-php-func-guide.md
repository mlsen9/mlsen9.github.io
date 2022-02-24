---
title: PHP 函数使用指南
date: 2022-02-23 19:00:00 +0800
categories: [Code, PHP]
tags: [php-func, func-guide]
---

### sprintf 格式化字符串
```php
/**
 * 控制浮点数打印格式
 * 浮点数使用格式符”%f”控制，默认保留小数点后6 位数字，
 * 自己控制打印的宽度和小数位数，这时就应该使用：”%m.nf”格式，
 * 其中m 表示打印的宽度，n 表示小数点后的位数。
 */

echo "\n控制总宽度：5位（小数点也包含，位数不够左边补0），保留2位小数。\n";
echo sprintf('%05.2f', 1);echo "\n";// 01.00
echo sprintf('%05.2f', 1.001);echo "\n";// 01.00
echo sprintf('%05.2f', 1.1234);echo "\n";// 01.12
echo sprintf('%05.2f', 1.4567);echo "\n";// 01.46
// 左边补 # 号
echo sprintf("%'#5.2f", 1.4567);echo "\n";// #1.46
// 不控制总宽度
echo sprintf('%.2f', 1.4567);echo "\n";// 1.46


// 不指定宽度：保留2位小数。
echo "\n不指定宽度：保留2位小数。\n";
echo sprintf('%.2f', 10);echo "\n"; // 10.00
echo sprintf('%.2f', 10.0);echo "\n"; // 10.00
echo sprintf('%.2f', 10.1001);echo "\n"; // 10.10
echo sprintf('%.2f', 10.1234);echo "\n"; // 10.12
echo sprintf('%.2f', 10.4567);echo "\n"; // 10.46


echo "\n指定填充字符\n";
echo sprintf("%'.9d\n", 123);// ......123
echo sprintf("%'.09d\n", 123);// 000000123
echo sprintf("1%'.05d\n", 12);// 100012，1 拼接 指定填充的5位，


echo "\n12位长度的百亿编号生成器（前缀为1）\n"; // 1 + 11位指定
echo sprintf("1%'.011d\n", 1);// 100000000001
echo sprintf("1%'.011d\n", 999);// 100000000999


echo "\n12位长度的百亿编号生成器（前缀为2）\n"; // 1 + 11位指定
echo sprintf("2%'.011d\n", 1);// 200000000001
echo sprintf("2%'.011d\n", 999);// 200000000999
echo sprintf("2%011d\n", 999);// 200000000999
echo sprintf("2%'#11d\n", 999);// 2########999
```

### 参考文献

> - [sprintf() 函数详解](https://www.runoob.com/php/func-string-sprintf.html){:target="_blank"}

