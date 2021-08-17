<template>
  <div class="card" :style="{height: data.height+'px'}">
    <div>
      {{ data.id }}
    </div>
    <div>{{ bigNumber }}</div>
  </div>
</template>

<script lang="ts">
import {defineComponent, computed, onMounted, onUpdated, onUnmounted} from 'vue'

export default defineComponent({
  name: 'Card',
  props: {
    data: {
      type: Object,
      default: () => {
      }
    }
  },
  setup: () => {

    function go(n:number) {
      var item = 0;      // 这里执行了一次
      for (var i = 0; i < n; i++) {   //这里执行了N次
        for (var j = 0; j < n; j++) {     //这里执行了n*n次
          item = item + i + j;     //这里执行了n*n次
        }
      }
      return item;  //这里执行了一次
    }

    const bigNumber = computed(()=> go(2000));

    return {
      bigNumber
    }
  }
})
</script>

<style scoped>
.card {
  outline: 1px solid red;
  word-break: break-all;
  min-height: 100px;
  margin-bottom: 8px;
}
</style>
