---
title: JavaScript 常用函数整理
date: 2022-02-13 11:53:33 +0800
categories: [Code, JavaScript]
tags: [js-func]
---

### 计算 BMI
```javascript
function computePatientBMI(height, weight) {
    var bmi = 0;
    if (height && weight) {
        bmi = weight / (Math.pow(height, 2) / 10000);
        bmi = Number(bmi).toFixed(1);
    }

    return bmi;
}
```

### 字符串截取
```javascript
// JS判断字符串长度（英文占1个字符，中文汉字占2个字符）
// JS根据字节截取字符串长度（英文占1个字符，中文汉字占2个字符）
String.prototype.gblen = function() {
    var len = 0;
    for (var i = 0; i < this.length; i++) {
        if (this.charCodeAt(i) > 127 || this.charCodeAt(i) == 94) {
            len += 2;
        } else {
            len++;
        }
    }

    return len;
}

/**
 * 根据字节截取字符串，中文视为2个字节，英文为1个字节
 */
function _mbSubstr(string, start, end) {
    var finalString = '';

    var words = [];
    for (var i = 0; i < string.length; i++) {
        // 如果是中文，在中文的前面push一个占位符
        if (string.charCodeAt(i) > 127 || string.charCodeAt(i) == 94) {
            words.push('');
            words.push(string[i]);
        } else {
            words.push(string[i]);
        }
    }

    var subArray = words.slice(start, end);
    finalString = subArray.join('', subArray);

    return finalString;
}

var string = new String('你好world');
var total = string.gblen();
console.log('长度为：' + total);
var str = _mbSubstr('你好world', 0, 5);
console.log(str);
```

### 秒数转为时分秒显示

```javascript
function secondsFormat(seconds) {
    var h = Math.floor(seconds / 3600);
    var m = Math.floor((seconds / 60 % 60));
    var s = Math.floor((seconds % 60));

    return result = h + "小时" + m + "分钟" + s + "秒";
}
```
