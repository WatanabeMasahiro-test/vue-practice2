<script setup lang="ts">
import { ref, onMounted } from 'vue'
import ComponentTest1 from './components/ComponentTest1.vue'
import axios from 'axios'
import {type Bpi, type DbJson} from './composables/types/interface'

const title = ref<string>('ここにタイトル')

const bpi = ref<Bpi[]>()
const bpiIsBoolean = ref<Boolean>()

// let dbJson = reactive({} as DbJson)
const dbJson = ref<DbJson[]>()
let dbJsonIsBoolean = ref<Boolean>()

const testMethod = () => alert('HELLO!')
const handleEvent = () => alert('子コンポーネントからの通知')

onMounted(() => {
  try {
    axios.get('https://api.coindesk.com/v1/bpi/currentprice.json')
    .then(function (response) {
      console.log(response.data.bpi) // デバッグ用にconsoleに出力
      bpi.value = response.data.bpi
    })
  } catch(error) {
      console.log(error)
      bpiIsBoolean.value = false
  }

  try {
    axios.get('/db.json')
    .then(function (response) {
      console.log(response.data.posts) // デバッグ用にconsoleに出力
      dbJson.value = response.data.posts
    })
  } catch(error) {
      console.log(error)
      dbJsonIsBoolean.value = false
  }
})
</script>

<template>
  <div>
    <img alt="Vue logo" src="./assets/logo.svg" width="300" height="200">
    <h1>{{ title }}</h1>
    <ComponentTest1 hello-msg="ハローメッセージ" @notification="handleEvent"/>
    <button @click="testMethod()">おためし</button>
    <hr class="double-line">
    <div class="bg-silver p-20px">
      <table v-if="bpiIsBoolean !== false">
        <tr>
          <th></th>
          <th v-for="i in 4" :key="i">{{ i }}</th>
        </tr>
        <tr
          v-for="(item, index) in bpi"
          :key="index"
        >
          <td>{{ item.code  }}</td>
          <td>{{ item.symbol }}</td>
          <td>{{ item.rate }}</td>
          <td>{{ item.description }}</td>
          <td>{{ item.rate_float }}</td>
        </tr>
      </table>

      <table v-if="dbJsonIsBoolean !== false">
        <tr>
          <th></th>
          <th v-for="i in 2" :key="i" class="w-100">{{ i }}</th>
        </tr>
        <tr
          v-for="item in dbJson"
          :key="item.id"
        >
          <th>{{ item.id }}</th>
          <td>{{ item.title }}</td>
          <td>{{ item.score }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<style scoped>
  h1 {
    text-align: center;
  }

  img, button {
    display: block;
    margin: 10px auto;
  }

  table, td, th {
    margin: 20px auto;
    border: 1px solid;
    background-color: ghostwhite;
  }

  th, td {
    padding: 2px 10px
  }

  .double-line {
    border-style: double;
  }

  .bg-silver {
    background-color: silver;
  }

  .p-20px {
    padding: 20px;
  }

  .w-100 {
    width: 100px;
  }

</style>
