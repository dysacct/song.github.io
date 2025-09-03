---
title: watch & watchEffect
date: 2025-08-31 14:01:16
categories: [vue]
---

### watch
<script setup lang="ts">
import { ref, watch, reactive } from 'vue';
// 情况一：监视ref 基本属性
let sum = ref(0);

function changeSum() {
  sum.value += 1;
}
// 监视

const stopWatch = watch(sum, (newValue, oldValue) => {
  console.log('变化了', newValue, oldValue);
  if (newValue >= 10) {
    stopWatch();
  }
});

// 情况二：监视ref 对象类型
let person = ref({
  name: '张三',
  age: 18,
});

function changeName() {
  person.value.name = '李四';
}
function changeAge() {
  person.value.age += 1;
}
function changePerson() {
  person.value = { name: '王五', age: 23 };
}
watch(
  person,
  (newValue, oldValue) => {
    console.log('变化了', newValue, oldValue);
  },
  { deep: true, immediate: true } // deep 就是显示深度值，immediate就是立即显示
); // 只有监视ref对象的地址值，需要监视对象内部属性的变化，需要手动开启深度监视
// watch 的第一个参数是被监视的数据，第二个参数是监控的回调，第三个参数是配置对象（deep,immediate...)

// 情况三: 监视reactive
// 监视reactive 对象类型，默认开启深度监视
let student = reactive({
  y_name: '小明',
  y_age: 13,
});
function ychangeName() {
  student.y_name = '小刚';
}
function ychangeAge() {
  student.y_age += 1;
}
function ychangePerson() {
  Object.assign(student, { y_name: '小蓝', y_age: 23 });
}
watch(student, (newValue, oldValue) => {
  console.log('变化了:', newValue, oldValue);
});

// 情况四：监视对象类型的某个属性/ 一个函数，返回一个内容
let teacher1 = reactive({
  tname: '黄老师',
  tage: 28,
  tcar: {
    c1: '奔驰',
    c2: '宝马',
  },
});

function ChangeTeacher() {
  teacher1.tname = '刘老师';
}
function ChangeAge() {
  teacher1.tage += 29;
}
function ChangeC1() {
  teacher1.tcar.c1 = '奥迪';
}
function ChangeC2() {
  teacher1.tcar.c2 = '大众';
}
function ChangeCar() {
  teacher1.tcar = { c1: '法拉利', c2: '特斯拉' };
}

watch(
  () => teacher1.tcar,
  (newValue, oldValue) => {
    console.log('teacher1变化了', newValue, oldValue);
  },
  { deep: true } // 函数返回使用deep
);
// 情况五：监视多个数据
let teacher2 = reactive({
  tname: '黄老师',
  tage: 28,
  tcar: {
    c1: '奔驰',
    c2: '宝马',
  },
});

function tchangeTeacher() {
  teacher2.tname = '刘老师';
}
function tchangeAge() {
  teacher2.tage += 29;
}
function tchangeC1() {
  teacher2.tcar.c1 = '奥迪';
}
function tchangeC2() {
  teacher2.tcar.c2 = '大众';
}
watch(
  [() => teacher2.tname, () => teacher.tcar.c1],
  (newValue, oldValue) => {
    console.log('tname和c1变化了', newValue, oldValue);
  },
  { deep: true }
);
</script>

<template>
  <div class="person">
    <!-- <h1>情况一: ref的基本类型</h1> -->
    <h2>当前求和:{{ sum }}</h2>
    <button @click="changeSum">点我sum+1</button>
  </div>
  <div class="person">
    <h2>姓名: {{ person.name }}</h2>
    <h2>年龄: {{ person.age }}</h2>
    <button @click="changeName">修改名字</button>
    <button @click="changeAge">修改年龄</button>
    <button @click="changePerson">修改整个人</button>
  </div>
  <div class="person">
    <h2>姓名: {{ student.y_name }}</h2>
    <h2>年龄: {{ student.y_age }}</h2>
    <button @click="ychangeName">修改名字</button>
    <button @click="ychangeAge">修改年龄</button>
    <button @click="ychangePerson">修改整个人</button>
  </div>
</template>

<style scoped>
.person {
  background: #ccc;
  padding: 100px 150px;
  box-shadow: 0 0 10px skyblue;
  border-radius: 10px;
  margin: auto;
}
</style>
```