# 键盘事件

# 简介

体验地址: http://demo.404mzk.com/event/base/keyboard.html

javascript主要涉及到的键盘事件的顺序为 keydown -> keypress -> keyup

 情况/事件| keydown | keypress | keyup
---- | --- | ---
按住某键后按其他健是否会继续触发 | true | false | false
忽略系统功能键(回退、shift等) |  false | true | false
区分大小写 | false | true | false
this.value可以包含刚按下的字符 | false | false | true
按键为非字符是否触发 | true | false | true

tips:

1. keyboard事件都会有shiftkey ctrlkey altkey matakey 代表这几个键是否被按住, matakey在window中代表win键, mac中代表command键, IE不支持matakey
2. keydown和keyup区分不出大小写, keycode都是大写的ASCII

# 常用例子

### 监听粘贴操作(组合键问题)

```javascript
var is_mac = navigator.userAgent.indexOf('Mac OS X') !== -1,
    is_window = (navigator.platform == "Win32") || (navigator.platform == "Windows")
    
listener(document.querySelector('.j_copy'), 'keydown', function(event){
    if ((is_mac && event.metaKey && getCode(event) === 86)
        || (is_window && event.ctrlKey && getCode(event) === 86)
    ){
        document.querySelector('.j_copy_ul').innerHTML += '<li>被粘贴辣</li>'
    }
 })
```

### 输入只能是整数

```javascript
var prev_string = '';
document.querySelector('.j_int_input_1').onkeyup = function () {
  if ( isNaN(this.value) ){
    this.value = prev_string
  }else if (this.value === '' ){

  }else {
    this.value = parseInt(this.value)
    prev_string = this.value
  }
 
}    
```

# TIPS

### keypress 和 keyup|keydown中keycode的区别

在字母数字等情况下 keypress和keyup|keydown的keycode是一致的

但是细分则不一致

keypress中表示的是键入的字符(character code)

keydown和keyup表示的是具体按下的健(virtual keycode)

比较经典的是`键(小键盘1左边的键)

keypress触发的keycode是96 

keydown|keyup触发的keycode是192

### keypress和keydown|keyup触发的情况的区别

keypress是有明确字符输入的时候才触发

而keydown|keyup则是按下键盘即会触发

# 兼容性

### 存放ASCII地方

1. IE9上event对象charCode 保存按键的ASCII
2. 高级浏览器只有在触发keypress使用包含charCode, 并且值和keycode一致
3. mac的firefox keydown和keyup用keyCode存放ASCII, keypress用charCode存放ASCII

所以建议这样取ASCII

```javascript
getCode: function(event) {
    if ( event.charCode) {
        return event.charCode;
    } else {
        return event.keyCode;
    }
}
```
