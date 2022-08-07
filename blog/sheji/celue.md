# 策略模式

## 基本思想

- 分出静态(context)
- 分出动态(策略对象)

---


## 表单验证

```javascript
/*
 *@Author: 赵元达
 *@Date: 2022-06-17 08:33:23
 *@parms:
 *@Description: 策略模式
 */

// 思想是把静态的和动态的分离开来 单独弄成两个类去实现它

// 例子 实现表单的验证

// 规则类 (处理表单验证规则的部分)
class Strategies {
    constructor(){}
    // 判断是否为空
    isNonEmpty(val,errMsg){
        if(val === ''){
            return errMsg
        }
    }
    // 限制最小长度
    minLen(val,len,errMsg){
        if(val.length < len){
            return errMsg
        }
    }
    // 判断手机号格式
    isMobile(val,errMsg){
        // 使用正则表达式
        if(!/(^1[3|5|8][0-9]{9}$)/.test(val)){
            return errMsg
        }
    }
}

// 逻辑策略类
class Validatar{
    cache = []
    strategies = {}
    constructor(){
        this.strategies = new Strategies()
    }
    // 后期可以考虑再抽离一下
    add(dom,rule,errMsg){
        let ary = rule.split(':')
        this.cache.push(()=>{ 
            // 获取到规则名
            let strategy = ary.shift()
            // 把dom的值添加到列表中
            ary.unshift(dom.value)
            ary.push(errMsg)
            // 改变this的指向
            return this.strategies[strategy].apply(dom,ary)
        })
    }
    // 开始
    start(){
        for(let i=0;i<this.cache.length;i++){
            let validatarFunc = this.cache[i]
            let msg = validatarFunc()
            // 说明没有通过校验通过
            if(msg){
                return msg
            }
        }
    }
}

// 静态部分（context部分）

class ValidatarHandle{
    validatar = {}
    errMsg = []
    constructor(){
        this.validatar = new Validatar()
    }

    handleAdd(dom,rule,errMsg){
        this.validatar.add(dom,rule,errMsg)
    }
    getErrMsg(){
        return this.validatar.start()
    }
}

// 开始处理了

// 获取dom
let ctn = document.getElementById('ctn')
let userName = document.getElementById('userName')
let password = document.getElementById('password')
ctn.onclick = function(){
    let valid = new ValidatarHandle()
    valid.handleAdd(userName,'isNonEmpty','名字不能为空')
    valid.handleAdd(password,'minLen:6','密码长度不能少于6位')
    let errMsg =  valid.getErrMsg()
    if(errMsg){
        alert(errMsg)
        // 阻止表单提交
        return false
    }
}

```

---