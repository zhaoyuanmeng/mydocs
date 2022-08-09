# 正则表达式


## 限制输入的长度并且必须是数字(利用keypress事件)

```javascript
// 传入的是输入框的值
  isNumber(value) {
      let evt = window.event
      if (value.length == 8) {
        evt.preventDefault()
      }
      var charCode = evt.which ? evt.which : evt.keyCode
      if (charCode > 31 && (charCode < 48 || charCode > 57) && charCode !== 46) {
        evt.preventDefault()
      } else {
        return true
      }
    },



```