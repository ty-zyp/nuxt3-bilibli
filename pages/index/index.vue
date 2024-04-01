<script setup lang="ts">
import type { RootObject } from '@/types/video' // mynuxtone\types\video.d.ts
// import { ref } from 'vue'

// const props = withDefaults(
//   defineProps<{
//     prop1?: string
//   }>(),
//   {
//     prop1: '',
//   }
// )

// const emit = defineEmits<{
//   (event: 'event1'): void
// }>()

const { data: channelList } = await useFetch('/api/channel')
const { data: videoList } = await useFetch('/api/video')
// console.log(1, videoList)

const list = ref<RootObject[]>([])
const loading = ref(false)
const finished = ref(false)

let page = 1
const pageSize = 20

const onLoad = () => {
  // console.log('加载load')
  // // loading.value = true
  // const data = videoList.value?.slice(
  //   (page - 1) * pageSize,
  //   page * pageSize
  // ) as VideoItem[]

  // list.value.push([...data])
  // page++

  // // 加载状态结束
  // loading.value = false

  // // 数据全部加载完成
  // if (list.value?.length >= videoList.value?.length) {
  //   finished.value = true
  // }
  // 表示正在加载
  loading.value = false
  // 根据当前页码提取数据
  const data = videoList.value?.slice(
    (page - 1) * pageSize,
    page * pageSize
  ) as RootObject[]
  // 追加到用于渲染的数组中
  list.value.push(...data)
  // 页码累加
  page++
  // 加载结束
  if (videoList.value?.length === list.value.length) {
    finished.value = true
  }
  console.log('data', list.value.length, list.value)
}

onLoad()
</script>

<template>
  <!-- 公共头部 -->
  <AppHeader />
  <!-- 频道模块 -->
  <van-tabs>
    <van-tab v-for="item in channelList" :key="item.id" :title="item.name" />
  </van-tabs>
  <!-- 视频列表 -->
  <van-list
    class="video-list"
    v-model:loading="loading"
    :finished="finished"
    finished-text="去 bilibili App 看更多"
    @load="onLoad"
  >
    <AppVideo :list="list" />
  </van-list>
  <!-- </div> -->
</template>

<style lang="scss">
// 视频列表
.video-list {
  display: flex;
  flex-wrap: wrap;
  padding: 10px 5px;
}
</style>
