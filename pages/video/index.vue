<script setup lang="ts">
import { ref } from 'vue'

const props = withDefaults(
  defineProps<{
    prop1?: string
  }>(),
  {
    prop1: '',
  }
)

const emit = defineEmits<{
  (event: 'event1'): void
}>()
</script>

<template>
  <div class="container">
    video
    <div class="box">hello</div>
  </div>
</template>

<style scoped lang="scss">
.container {
  color: red;

  .box {
    color: yellow;
  }
}
</style>
