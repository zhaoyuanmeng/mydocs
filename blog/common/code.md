# 常用utils工具函数

- 一维数组转树

```javascript
// 后端需要传过来的一维数组数据
// var data1 = [
//   { id: 40, parentId: 0, note: "一级菜单-1" },
//   { id: 20, parentId: 0, note: "一级菜单-2" },
//   { id: 22, parentId: 20, note: "二级菜单-22" },
//   { id: 24, parentId: 22, note: "三级菜单-24" },
//   { id: 34, parentId: 22, note: "三级菜单-34" },
// ];
export const useArrayToTree = (data, parent) => {
  var tree = []
  var temp
  for (var i = 0; i < data.length; i++) {
    if (data[i].partentId == parent) {
      var obj = data[i]
      temp = useArrayToTree(data, data[i].equId)
      if (temp.length > 0) {
        obj.children = temp
      }
      tree.push(obj)
    }
  }
  return tree
}

```

---

- 深拷贝

```javascript
/*
 *@Author: 赵元达
 *@Date: 2022-09-07 15:22:17
 *@parms:
 *@Description: 深拷贝
 */
export function deepClone(target) {
  // 定义一个变量
  let result
  // 如果当前需要深拷贝的是一个对象的话
  if (typeof target === 'object') {
    // 如果是一个数组的话
    if (Array.isArray(target)) {
      result = [] // 将result赋值为一个数组，并且执行遍历
      for (const i in target) {
        // 递归克隆数组中的每一项
        result.push(deepClone(target[i]))
      }
      // 判断如果当前的值是null的话；直接赋值为null
    } else if (target === null) {
      result = null
      // 判断如果当前的值是一个RegExp对象的话，直接赋值
    } else if (target.constructor === RegExp) {
      result = target
    } else {
      // 否则是普通对象，直接for in循环，递归赋值对象的所有值
      result = {}
      for (const i in target) {
        result[i] = deepClone(target[i])
      }
    }
    // 如果不是对象的话，就是基本数据类型，那么直接赋值
  } else {
    result = target
  }
  // 返回最终结果
  return result
}


```

---

- 时间格式化处理

```javascript

/**
 * @author Zhao YuanDa
 * @parms: 
 * @description: //获取当前时间
 * @date 2022-09-18 09:20
 */
export function CurentTime() {
  let now = new Date()
  let year = now.getFullYear() //年
  let month = now.getMonth() + 1 //月
  let day = now.getDate() //日
  let hh = now.getHours() //时
  let mm = now.getMinutes() //分
  let clock = year
  if (month < 10) clock += '0'
  clock += month
  if (day < 10) clock += '0'
  clock += day
  if (hh < 10) clock += '0'
  clock += hh
  if (mm < 10) clock += '0'
  clock += mm
  return clock
}

```

---

- 文件导出

```javascript

// 通过文件流的方式
export function exportExcelByFilePlus(url, params, filename) {
  axios({
    url: url,
    method: 'post',
    data: params,
    responseType: 'blob',
    headers: {
      type: 'multipart/form-data'
    }
  }).then(res => {
    let content = res // 文件流
    if (filename) {
      myResolveBlob(content, filename)
    } else {
      myResolveBlob(content, 'test')
    }
  })
}

/**
 * 自己的通用下载方法
 * @param {Object} res 数据流
 * @param {String} fileName 文件名字
 */
export function myResolveBlob(res, fileName) {
  const aElement = document.createElement('a')
  const blob = new Blob([res], { type: 'application/vnd.ms-excel,charset=utf-8' })
  const url = window.URL.createObjectURL(blob)
  aElement.setAttribute('href', url)
  aElement.setAttribute('download', fileName)
  document.body.appendChild(aElement)
  aElement.click()
  window.URL.revokeObjectURL(url)
  document.body.removeChild(aElement)
}
/**
 * @author Zhao YuanDa
 * @parms: 
 * @description: //zip下载
 * @date 2022-09-18 09:38
 */
export function myResolveBlobZip(res, fileName) {
  const aElement = document.createElement('a')
  const blob = new Blob([res], { type: 'application/zip,charset=utf-8' })
  const url = window.URL.createObjectURL(blob)
  aElement.setAttribute('href', url)
  aElement.setAttribute('download', fileName)
  document.body.appendChild(aElement)
  aElement.click()
  window.URL.revokeObjectURL(url)
  document.body.removeChild(aElement)
}

```

---

- 文件解析

```javascript

```

---

- 防抖

```javascript

/**
 * @desc 函数防抖
 * @param func 函数
 * @param wait 延迟执行毫秒数
 * @param immediate true 表立即执行，false 表非立即执行
 */
export const debounce = (func, wait, immediate) => {
  let timeout
  return function() {
    let context = this
    let args = arguments

    if (timeout) clearTimeout(timeout)
    if (immediate) {
      var callNow = !timeout
      timeout = setTimeout(() => {
        timeout = null
      }, wait)
      if (callNow) func.apply(context, args)
    } else {
      timeout = setTimeout(function() {
        func.apply(context, args)
      }, wait)
    }
  }
}

```

---

- 节流 

```javascript
/**
 * @author Zhao YuanDa
 * @parms: 
 * @description: //节流
 * @date 2022-09-18 09:22
 */
export function throttle(fun, delay) {
  // 这里相当于是下面那个函数的上层环境 是能够访问到的  作用域链的知识点了 闭包
  let last, deferTimer;
  return function (args) {
    let that = this;
    let _args = arguments;
    let now = +new Date();
    if (last && now < last + delay) {
      clearTimeout(deferTimer);
      deferTimer = setTimeout(function () {
        last = now;
        fun.apply(that, _args);
      }, delay);
    } else {
      last = now;
      fun.apply(that, _args);
    }
  };
}


```

---

- 对象深度合并

```javascript
/**
 * @author Zhao YuanDa
 * @parms: 多个需要合并的对象
 * @description: //深度合并对象的
 * @date 2022-09-16 16:47
 */
export function deepAssign(...param: any) {
  // 让这个去做核心处理 assign回去筛选
  let result = Object.assign({}, ...param);
  for (let item of param) {
    for (let [idx, val] of Object.entries(item)) {
      if (typeof val === 'object') {
        result[idx] = deepAssign(result[idx], val);
      }
    }
  }
  return result;
}

```

---

- axios基础封装

```javascript
// 初始化
const service = axios.create({
  baseURL: baseURL, // api base_url
  timeout: 600000 // 请求超时时间
})

// 设置报错的回调处理函数
const errCallback = error => {
  if (error.response) {
    const data = error.response.data
    const token = Vue.ls.get(ACCESS_TOKEN)
    if (error.response.status === 403) {
      notification.error({
        message: 'Forbidden',
        description: data.message
      })
    }
    if (error.response.status === 401 && !(data.result && data.result.isLogin)) {
      notification.error({
        message: 'Unauthorized',
        description: 'Authorization verification failed'
      })
      if (token) {
        store.dispatch('Logout').then(() => {
          setTimeout(() => {
            window.location.reload()
          }, 1500)
        })
      }
    }
  }
  return Promise.resolve(error.response || {})
}

// 设置请求拦截器
service.interceptors.request.use(config => {
  // 这里拿到token
  const token = Vue.ls.get(ACCESS_TOKEN)
  if (token) {
    config.headers['token'] = token // 让每个请求携带自定义 token 请根据实际情况自行修改
  }
  return config
}, errCallback)

// 设置响应拦截器
service.interceptors.response.use(response => {
  return response.data
}, errCallback)

export default service

// === 其他地方引入

import service from "./axios";

export const getRequest = (url, parameter = {}) => {
  return service({
    url: url,
    method: 'get',
    params: parameter
  })
}

export const postRequest = (url, parameter = {}) => {
  return service({
    url: url,
    method: 'post',
    params: parameter
  })
}

export const postBlobRequest = (url, parameter = {}) => {
  return service({
    url: url,
    method: 'post',
    data: parameter,
    responseType: 'blob',
    headers: {
      'Content-Type': 'application/json'
    }
  })
}




```

---