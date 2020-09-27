<template>
  <div>
    <p>{{state.time}}</p>
    <button @click="myFn">按钮</button>
  </div>
</template>

<script>
  /*
  1.什么是reactive?
    - reactive是Vue3中提供的实现响应式数据的方法
    - 在Vue2中响应式数据是通过defineProperty来实现的
      而在Vue3中响应式数据是通过ES6的Proxy来实现的
  2.reactive注意点:
    - reactive参数必须是对象(json/arr)
    - 如果给reactive传递了其它对象
      + 默认情况下修改对象, 界面不会自动更新
      + 如果想更新, 可以通过重新赋值的方式
  * */
  import {reactive} from 'vue';
export default {
  name: 'App',
  setup() {
    // setup函数里的this为undefined是因为严格模式下this的指向不会隐式指向全局对象，而是undefined，并不是vue刻意把它修改为undefined不让使用。
     console.log(this);
    // 创建一个响应式数据
    // 本质: 就是将传入的数据包装成一个Proxy对象
    // let state = reactive(123);
    // let state = reactive({
    //   age: 123
    // });
    // let state = reactive([1, 3, 5]);
    let state = reactive({
      time: new Date(),
      goods:[1,2,3]
    });
    function myFn() {
     // state = 666; // 由于在创建响应式数据的时候传递的不是一个对象, 所以无法实现响应式
     // state.age = 666;
     // state[0] = 666;
     // 直接修改以前的, 界面不会更新
      state.time.setDate(state.time.getDate() + 1);
      // 重新赋值
      const newTime = new Date(state.time.getTime());
      newTime.setDate(state.time.getDate() + 1);
      state.time = newTime;
      console.log(state.time);
      console.log(state.goods);
    }
    return {state, myFn}; 
  }
}
</script>

<style>

</style>
