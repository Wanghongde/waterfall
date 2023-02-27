<template>
  <div class="water-fall">
    <div
      class="water-fall-item"
      v-for="item in waterList"
      :style="{
        height: item.height + 'px',
        top: item.top + 'px',
        left: item.left + 'px',
      }"
    >
      <img :style="{height: item.height + 'px', width: '120px'}" :src="item.images" alt="" />
    </div>
  </div>
</template>

<script setup lang="ts">
import axios from '@/utils/axios.js'
import { ref, reactive, onMounted } from 'vue'

interface item {
  images: string,
  height: number,
  top?:number,
  left?:number,
}

let list = reactive<item[]>([])
let waterList = reactive<item[]>([])

function getRandomInt(min:number, max:number) {
  min = Math.ceil(min)
  max = Math.floor(max)
  return (Math.floor(Math.random() * (max - min + 1)) + min) * 50
}

let getList = async () => {
  let {data: {data: { results } }} = await axios.get('http://geek.itheima.net/v1_0/articles?channel_id=0&timestamp=1654835148606')

  let arr: any = results.map((item: any) => (item.cover.images ? item.cover.images : []))
    .flat()
    .map((item: any) => ({images: item, height: getRandomInt(1, 10) }))

  let arr2: any = results.map((item: any) => (item.cover.images ? item.cover.images : []))
    .flat()
    .map((item: any) => ({images: item, height: getRandomInt(1, 10) }))

  let arr3: any = results.map((item: any) => (item.cover.images ? item.cover.images : []))
    .flat()
    .map((item: any) => ({images: item, height: getRandomInt(1, 10) }))

  list.push(...arr, ...arr2, ...arr3)
}

const init = () => {
  const heightList: any[] = []
  const width = 130
  const x = document.body.clientWidth
  const column = Math.floor(x / width)

  for (let i = 0; i < list.length; i++) {
    if (i < column) {
      list[i].top = 10
      list[i].left = i * width
      heightList.push(list[i].height + 10)
      waterList.push(list[i])
    } else {
      let current = heightList[0]
      let index = 0
      heightList.forEach((h, inx) => {
        if (current > h) {
          current = h
          index = inx
        }
      })
      list[i].top = current + 20
      console.log(list[i].top, "top", i)
      list[i].left = index * width
      heightList[index] = heightList[index] + list[i].height + 20
      waterList.push(list[i])
    }
  }
}

onMounted(async () => {
  window.onresize = () => init()
  await getList()
  init()
})
</script>

<style scoped lang="less">
.water-fall {
    position: relative;
     height: 100%;
    .water-fall-item {
        position: absolute;
        width: 120px;
    }
}
</style>
