---
title: PHP 常用函数整理
date: 2022-02-23 19:00:00 +0800
categories: [Code, PHP]
tags: [php-func]
---

### 将图片转为 base64 格式
```php
/**
 * 将图片转为 base64 格式
 * @param string $image
 * @return bool|string
 */
function imageTobase64($image)
{
    if (empty($image)) {
        return false;
    }
    $pos = strrpos($image, '.');
    if ($pos === false) {
        return false;
    }
    $extension = strtolower(substr($image, $pos + 1));
    if (!in_array($extension, ['png', 'jpg', 'jpeg', 'gif'])) {
        return false;
    }
    // 判断 [远程图片地址|本地图片地址] 格式是否正确
    $mimeCode = @exif_imagetype($image);
    switch ($mimeCode) {
        case IMAGETYPE_GIF:
            $mimeType = 'image/gif';
            break;
        case IMAGETYPE_JPEG:
            $mimeType = 'image/jpeg';
            break;
        case IMAGETYPE_PNG:
            $mimeType = 'image/png';
            break;
        default:
            return false;
            break;
    }
    $content = file_get_contents($image);
    if (!$content) {
        return false;
    }
    $fileContent   = base64_encode($content);
    $fileContent   = chunk_split($fileContent);
    $dataUrlString = 'data:' . $mimeType . ';base64,' . $fileContent;

    return $dataUrlString;
}
image2base64('https://www.example.com/a.png');
image2base64('a.gif');
```

### 生成唯一的 GUID
```php
/**
 * GUID 的格式为“xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx”，
 * 其中每个 x 是 0-9 或 a-f 范围内的一个4位十六进制数。
 * 例如：6F9619FF-8B86-D011-B42D-00C04FC964FF 即为有效的 GUID 值。
 * @return string
 */
function gen_guid()
{
    $uniqid = uniqid(mt_rand(), true);
    $charid = strtolower(md5($uniqid));
    // 8位
    $stringA = substr($charid, 6, 2) . substr($charid, 4, 2) . substr($charid, 2, 2) . substr($charid, 0, 2);
    // 4位
    $stringB = substr($charid, 10, 2) . substr($charid, 8, 2);
    $stringC = substr($charid, 14, 2) . substr($charid, 12, 2);
    $stringD = substr($charid, 16, 4);
    // 12位
    $stringE = substr($charid, 20, 12);
    $guid    = $stringA . '-' . $stringB . '-' . $stringC . '-' . $stringD . '-' . $stringE;

    return strtoupper($guid);
}
```

### [按照比例随机分配](https://gist.github.com/helloxxb/b408ed6c0fc4252ae10c04a6653cf6a2){:target="_blank"}

### [HTTP 文件断点续传](https://gist.github.com/helloxxb/3a8441b0773983cb2075b68cb6cf49d4){:target="_blank"}

### [将日期格式化为易读的](https://gist.github.com/helloxxb/e3b99ed4a68e90e32c4bdf908e423063){:target="_blank"}
