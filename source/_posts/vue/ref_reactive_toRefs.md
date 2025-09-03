---
title: 响应式对象
date: 2025-08-31 13:57:59
categories: [Vue]
---

# 响应式对象

### ref

```vue
<script setup lang="ts" name="own123">
import { ref } from "vue";
// ref 是一个函数，返回一个基本类型的响应式对象

let name = ref("张三"); // 响应式数据
let age = ref(18);
let tel = "13888888888";
let address = "北京";
// const fixName = () => {name = '李四'}

// 方法
function ChangeName() {
  name.value = "李四"; // .value 是访问响应式对象的方法
}
function addAge() {
  age.value += 1;
}
function telNumber() {
  alert(tel); // alert 是一个提醒框
}
</script>

<template>
  <div class="own">
    <h2>姓名: {{ name }}</h2>
    <h2>年龄: {{ age }}</h2>
    <h2>地址: {{ address }}</h2>
    <button @click="ChangeName">修改名字</button>
    <button @click="addAge">增加年龄</button>
    <button @click="telNumber">查看联系方式</button>
  </div>
</template>

<style scoped>
.own {
  background: #ccc;
  padding: 20px;
  box-shadow: 0 0 10px skyblue;
  border-radius: 10px;
  margin: auto;
}
</style>
```

### reactive

```Vue
<script setup lang="ts">
import { reactive } from 'vue'; // 导入 reactive 方法, 用于只能创建响应式对象和ref的区别就是，ref是单个值，reactive是对象
// 数据
let car = reactive({ brand: '奔驰', price: 100 })
let games = reactive([{ id: '1', name: '王者荣耀' }, { id: '2', name: '英雄联盟' }, { id: '3', name: '和平精英' }])
let obj = reactive({ a: { b: { c: 100 } } })
// 方法
function changeGame1() {
  games[0].name = '和平精英'
}
function changeCar() {
  Object.assign(car, { brand: '兰博基尼', price: 300 }) // Object.assign() 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。
}
function changeName() {
  car.brand = '宝马'
}
function changePrice() {
  car.price += 10 // 修改响应式对象中的属性，会触发视图更新
}
function abc() {
  obj.a.b.c = 200
}
</script>

<template>
  <div class="student">
    <h2>汽车信息{{ car.brand }}, 价值{{ car.price }}</h2>
    <button @click="changePrice">涨价</button>
    <button @click="changeName">换车</button>
    <button @click="changeCar">换整辆车</button>
    <!-- <br /> -->
    <h2>游戏列表</h2>
    <ul>
      <li v-for="item in games" :key="item.id">{{ item.name }}</li> <!-- v-for 循环遍历 -->
    </ul>
    <button @click="changeGame1">修改游戏1</button>
    <h2>测试{{ obj.a.b.c }}</h2>
    <button @click="abc">测试 </button>
  </div>

</template>

<style scoped>
.student {
  background: #ccc;
  padding: 20px;
  box-shadow: 0 0 10px skyblue;
  border-radius: 10px;
  margin: auto;
}

li {
  font-size: bolder;
}
</style>
```

### 两者区别

```vue
<script setup lang="ts">
import { ref } from "vue"; // ref 也可以使用，但是 ref 是一个函数，返回一个基本类型的响应式对象 { ref, reactive } from 'vue'
// 数据
let car = ref({ brand: "奔驰", price: 100 });
let sum = ref(0);
// 方法

function changeName() {
  // car.brand = '宝马'
  car.value.brand = "宝马";
  // console.log(car)
}
function changePrice() {
  car.value.price += 10; // 修改响应式对象中的属性，会触发视图更新
}
function changeCar() {
  // car = { brand: '兰博基尼', price: 300 }  页面是不更新的
  // Object.assign(car, { brand: '兰博基尼', price: 300 }) // 修改响应式对象中的属性，会触发视图更新
  // 用ref的话 car.value = { brand: '兰博基尼', price: 300 }
  car.value = { brand: "兰博基尼", price: 300 };
}
function changeSum() {
  sum.value += 1;
}
</script>

<template>
  <div class="student">
    <h2>汽车信息{{ car.brand }}, 价值{{ car.price }}</h2>
    <button @click="changePrice">涨价</button>
    <button @click="changeName">换车</button>
    <button @click="changeCar">换车2</button>
    <hr />
    <h2>测试求和{{ sum }}</h2>
    <button @click="changeSum">+1</button>
  </div>
</template>

<style scoped>
.student {
  background: #ccc;
  padding: 20px;
  box-shadow: 0 0 10px skyblue;
  border-radius: 10px;
  margin: auto;
}

li {
  font-size: bolder;
}
</style>
```
### toRefs
```vue
<script setup lang="ts">
// 数据
import { reactive, toRefs } from 'vue'
let student = reactive({
  name: '张三',
  age: 18,
})
let { name, age } = toRefs(student) // toRefs 不仅可以解构拿出数据，还可以作为响应式数据
console.log(name)
console.log(age)

// 方法
function changeName() {
  name.value += '~'
}
function addAge() {
  age.value += 1
}

</script>

<template>
  <div class="student">
    <h2>姓名: {{ name }}</h2>
    <h2>年龄: {{ age }}</h2>
    <button @click="changeName">修改名字</button>
    <button @click="addAge">增加年龄</button>
  </div>

</template>

<style scoped></style>
```