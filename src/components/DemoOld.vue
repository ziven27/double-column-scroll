<template>
  <dual-column>
    <Card v-for="item in data" :key="item?.id" :data="item"/>
  </dual-column>
</template>

<script lang="ts">
import {defineComponent, nextTick, onBeforeUpdate, onUnmounted, onUpdated, PropType} from 'vue'
import Card from "./Card.vue";
import DualColumn from "./DualColumn.vue";

export default defineComponent({
  name: 'DemoOld',
  components: {
    Card,
    DualColumn
  },
  props:{
    data:{
      type: Array as PropType<any[]>,
      default:()=>{}
    }
  },
  setup: () => {
    let counter = 0;
    console.log('demo old setUp');
    onBeforeUpdate(()=>{
      counter++;
      // console.log('demo old onBeforeUpdate');
      console.time(`old ${counter}`);
    })
    onUpdated(()=>{
      nextTick(function () {
        console.timeEnd(`old ${counter}`);
      });
    })
    onUnmounted(()=>{
      console.log('demo old onUnmounted');
    })
  }
})
</script>