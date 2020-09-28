<template>
  <div>
    <p>{{age}}</p>
    <button @click="myFn">按钮</button>
    <input type="text" v-model="text" @click="myFn">
  </div>
</template>

<script>
  /*
  1.customRef
  返回一个ref对象,可以显式地控制依赖追踪和触发响应
  * */
  import {ref, customRef} from 'vue';

function myRef(value) {
  return customRef((track, trigger)=>{
    return {
      get(){
        track(); // 告诉Vue这个数据是需要追踪变化的
        console.log('get', value);
        return value;
      },
      set(newValue){
        console.log('set', newValue);
        value = newValue;
        trigger(); // 告诉Vue触发界面更新
      }
    }
  });
}
function otherRef(val, delay=300) {
  let timeOut
  return customRef((track,trigger)=>{
    return {
      get() {
        track()
        return val
      },
      set(newVal){
        clearTimeout(timeOut)
        timeOut=setTimeout(() => {
          val= newVal
        }, delay);
        trigger()
      }

    }
  })
}
export default {
  name: 'App',
  setup() {
    // let age = ref(18); // reactive({value: 18})
    let age = myRef(18);
    let text =otherRef(123)
    function myFn() {
      age.value += 1;
      text.value = 6666
    }
    return {age, myFn,text}
  }
}
</script>

<style>

</style>
