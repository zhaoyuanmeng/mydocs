# ðº æåç³»å
- [èæåè¡¨](#ð»-èæåè¡¨)
---
- ## ð» æåpromise

```javascript

// è¿ä¸ªå°±æ¯åè°å½æ°çå®ç¾åºç¨
class MyPromise{
  // ç§æå±æ§

  // ç»å¼
  PromiseResult
  // ç¶æ
  PromiseState

  // ä¿å­æåçåè°å½æ°
  sucessFns
  // ä¿å­å¤±è´¥çåè°å½æ°
  errFns

  constructor(exec){
    // è¿éexecæ¯ä¸ä¸ªå½æ° è¿ä¸ªå½æ°è¦ä¸¤ä¸ªåæ° è¿ä¸¤ä¸ªåæ°ä¹æ¯å½æ°

    // ç»å®å¥½this
    this.initBind()
    // åå§åå¼
    this.initVal()
    exec(this.resolve,this.reject)
  }
  // å¤çthisé®é¢ç
  initBind(){
    this.resolve =  this.resolve.bind(this)
    this.reject =  this.reject.bind(this)
  }
  // åå§åç
  initVal(){
    this.PromiseResult = null
    this.PromiseState = 'pending'
  }
  // æ ¸å¿å½æ°
  resolve(val){
    // è¯´ææ¯ç­å¾ç¶æ
    if(this.PromiseState !== 'pending'){
      return
    }
    this.PromiseState = "fulfilled";
    this.PromiseResult = val;
    // è¯´æå½æ¶å­è´®äºå¼æ­¥çæä½
    if (this.sucessFns.length > 0) {
      // æ§è¡ç¸å½äºthenéé¢çæåå½æ°
      this.sucessFns.map((fn) => fn(this.PromiseResult));
    }

  }
  reject(reason){
    // è¯´ææ¯ç­å¾ç¶æ
     if(this.PromiseState !== 'pending'){
      return
    }
    this.PromiseState = "rejected";
    this.PromiseResult = reason;
    if (this.errFns.length > 0) {
      // æ§è¡ç¸å½äºthenéé¢çæåå½æ°
      this.errFns.map((fn) => fn(this.PromiseResult));
    }

  }
  // è¿éä¹æ¥åä¸¤ä¸ªåæ° é½æ¯å½æ°åæ°
  then(onFulFilled,onRejected = () => { }){
    if(typeof onFulFilled != 'function')return
    if(typeof onRejected != 'function')return

    // å¦ææå¼æ­¥çæåµ
    if(this.PromiseState == 'pending'){
       this.sucessFns.push(onFulFilled);
       this.errFns.push(onRejected);
       return;
    }
     // æå
    if (this.PromiseState == "fulfilled") {
      onFulFilled(this.PromiseResult);
      return;
    }
    // å¤±è´¥
    if (this.PromiseState == "rejected") {
      onRejected(this.PromiseResult);
      return;
    }

  }
  catch(){}
}

// æµè¯ 
let p1 = new MyPromise((resolve, reject) => {
  // è¿éé¢çåæ°å®éä¸æ¯ä¸ä¸ªå½æ° çæ­£å®ä¹è¿ä¸ªå½æ°çé»è¾æ¯æ¾å¨äºpromiseé£ä¸ªç±»éé¢
  // reject("å¤±è´¥äºï¼ï¼ï¼");
  // resolve("æåäºï¼ï¼ï¼ï¼");
  setTimeout(() => {
    resolve("æåäºï¼ï¼ï¼ï¼");
  }, 1000);
});
p1.then(
  (res) => {
    console.log("res", res);
  },
  (err) => {
    console.log("erro", err);
  }
);

```

---
- ## ð» newå®ç°çåç

```javascript
function Person(name){
  this.name =name
  this.sayName =function(){
    console.log('1111')
  }
}

// myNewçå®ç°
function myNew(fn,...args) {
  // obj.__proto__ = fn.prototype
  let obj = Object.create(fn.prototype)

  let res = fn.call(obj,arg)
  // è¿åå¼ ä¸å®æ¯ä¸ä¸ªå¯¹è±¡
  return typeof res == 'object' ? res : obj
}
let ceshi = myNew(Person,'liming')

```



---
- ## ð» æºç éé¢ç¼å­çå®ç°åç

```javascript

// ä¸»è¦æ¯å©ç¨äºevent loop
// è¿éä¹å¯ä»¥ç¨é­åå å°±æ¯åä¸ªå¨å±åé
let cbs = [];
let wating = false;
function nextTickPlus(cb) {
  cbs.push(cb);
  if (!wating) {
    // å¼æ­¥æ´æ° ä¸æ¯æä¸æ¬¡æ´æ°ä¸æ¬¡
    setTimeout(flushCallBacks, 0);
    wating = true;
  }
}
function flushCallBacks() {
  // è¿ä¸ªcbsä¼å®æ¶çæ¹å å¦æææ¹åçæåµä¸
  cbs.forEach((cb) => cb());
  wating = false;
}

```


---
- ## ð» bind call apply

```javascript

// å®ç°apply

function apply(context, args){
  //è¿éé»è®¤ä¸ä¼ å°±æ¯ç»window,ä¹å¯ä»¥ç¨es6ç»åæ°è®¾ç½®é»è®¤åæ°
  context = context || window
  args = args ? args : []
  //ç»contextæ°å¢ä¸ä¸ªç¬ä¸æ äºçå±æ§ä»¥åè¦çåæå±æ§
  const key = Symbol()
  context[key] = this
  //éè¿éå¼ç»å®çæ¹å¼è°ç¨å½æ°
  context[key](...args)
   //å é¤æ·»å çå±æ§
  delete context[key]
  //è¿åå½æ°è°ç¨çè¿åå¼
  return result
}


```

- ## ð» èæåè¡¨

```javascript
<template>
  <h1>æµè¯èæåè¡¨{{}}</h1>
  <div
    ref="demo"
    class="scroll-box demo"
    :style="`height: ${showNumber * itemHeight}px;`"
  >
    <!-- è¿ä¸ªæ¯ç¨æ¥ææ»å¨çææç -->
    <div
      class="scroll-blank"
      :style="`height: ${data.length * itemHeight}px;`"
    ></div>
    <div class="scroll-data" :style="`top: ${positionTop}px;`">
      <div v-for="(item, index) in activeList" :key="item" class="scroll-item">
        {{ item }}
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent, computed, onMounted, onUnmounted, ref } from 'vue';
import { useDebounceFn, useThrottleFn } from '@vueuse/core';

export default defineComponent({
  name: 'VueUse',
  setup() {
    //  scrollTopå¨ææ»å¨æ¡çæåµä¸ï¼ä¸ºåç´ å¯è§åºåè·ç¦»åç´ é¡¶é¨çåç´ ï¼ä¹å°±æ¯å·²ç»æ»å¨äºå¤å°è·ç¦»

    // è®¾è®¡æè·¯
    // çå¬æ»è½®äºä»¶/è§¦æ¸äºä»¶ï¼è®°å½åè¡¨çæ»åç§»éã
    // æ ¹æ®æ»åç§»éè®¡ç®åè¡¨çå¯è§åç´ èµ·å§ç´¢å¼ã
    // ä»èµ·å§ç´¢å¼æ¸²æåç´ è³è§å£åºé¨ã
    // å½æ»åç§»éæ´æ°æ¶ï¼éæ°æ¸²æå¯è§åç´ åè¡¨ãï¼å¯ä»¥ç¨computedè®¡ç®ï¼
    // ä¸ºå¯è§åç´ åè¡¨ååå å¥ç¼å²åç´ ã
    // å¨æ»å¨éæ¯è¾å°æ¶ï¼ç´æ¥ä¿®æ¹å¯è§åç´ åè¡¨çåç§»éã
    // å¨æ»å¨éæ¯è¾å¤§æ¶ï¼æ¯å¦æå¨æ»å¨æ¡ï¼ï¼ä¼éæ°æ¸²ææ´ä¸ªåè¡¨ã
    // äºä»¶èæµã

    const createData = (length: number) => {
      return Object.keys(new Array(length).fill(''));
    };

    const demo: any = ref(null); // å¤æ¡çå­
    const showNumber = 20; // å½åè§çªå±ç¤ºæ¡æ°
    const itemHeight = 20; // æ¯ä¸æ¡åå®¹çé«åº¦
    const data = createData(1000); // å®éæ°æ®
    let startNum = ref(0); // å½åè§çªèå´åç¬¬ä¸ä¸ªåç´ ä¸æ 
    let positionTop = ref(0); // å½åè§çªèå´åç¬¬ä¸ä¸ªåç´ åç§»é

    // è®¡ç®å½åè§çªåå®éè¦æ¸²æçåå®¹
    const activeList = computed(() => {
      const start = startNum.value;
      return data.slice(start, start + showNumber);
    });

    // æ»å¨çæ¶åè®¡ç®å½åè§çªèå´åç¬¬ä¸ä¸ªåç´ ä¸æ 
    // å ä¸ªä¼å èæµ
    const scrollEvent = (event: any) => {
      const { scrollTop } = event.target;
      startNum.value = parseInt(scrollTop / itemHeight + '');
      positionTop.value = scrollTop;
      console.log('positionTop', positionTop.value);
      console.log('startNum', startNum.value);
    };
    // é²æçç»æ
    const debouncedFn = useDebounceFn(scrollEvent, 100);

    // èæµçç»æ
    const throttleFn = useThrottleFn(scrollEvent, 100);
    onMounted(() => {
      demo.value?.addEventListener('scroll', throttleFn);
    });
    onUnmounted(() => {
      if (!demo.value) return;
      demo.value?.removeEventListener('scroll', throttleFn);
      demo.value = null;
    });

    return {
      showNumber,
      itemHeight,
      demo,
      positionTop,
      data,
      activeList,
    };
  },
});
</script>

<style>
.scroll-box {
  position: relative;
  overflow: auto;
  width: 400px;
  border: 1px solid rgb(0, 0, 0);
}
.scroll-data {
  position: absolute;
  background-color: aqua;
  width: 100%;
}
.scroll-item {
  height: 20px;
}
.scroll-item:hover {
  background: rgb(104, 111, 211);
  color: #fff;
}
</style>

```
[åèèµæ][https://juejin.cn/post/6844904183582162957#heading-3]

