---
title: JavaScript 对象封装示例
date: 2022-02-13 11:53:33 +0800
categories: [Code, JavaScript]
tags: [js-class, js-func]
---

### 示例1
```javascript
var Circle = (function () {
   function Circle() {
   }
   Circle.prototype.draw = function () {
      console.log("Cirlce is drawn");
   };
   return Circle;
})();
exports.Circle = Circle;
```

### 示例2
```javascript
(function(global, factory) {
    if (typeof define === 'function' && define.amd) {
        define([], factory);
    } else if (typeof module !== 'undefined' && module.exports) {
        module.exports = factory();
    } else {
        global.ObjectExample = factory();
    }
})(this,
function(options) {
    /**
     * @param {object} options
     * @constructor
     */
    function ObjectExample(options) {
        var _this = this;
        var settings = {
            selector: '',
            csrf_token: '',
            actions: [],
        };

        if (!options) {
            options = {};
        }

        /**
         * 删除一条记录
         */
        function deleteOne(element) {
            var url = $(element).attr('data-url') || '';
            var csrf_token = settings.csrf_token;
            // do something
        }

        this.setOptions = function(options) {
            for (var key in options) {
                if (typeof settings[key] !== 'undefined') {
                    settings[key] = options[key];
                }
            }
        }

        this.listenEvents = function() {
            var selector = settings.selector;
            var actions = settings.actions;
            if (actions.indexOf('deleteOne') !== -1) {
                console.log('deleteOne');
                $(selector).find('.dropdown-menu .delete-one-btn').on('click',
                function(event) {
                    deleteOne(this);
                });
            }
        }

        this.setOptions(options);
    }

    return ObjectExample;
});
```
