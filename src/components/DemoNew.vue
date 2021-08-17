<template>
  <List :data="data" #="{ item }" :get-item-height="getHeight" >
    <Card :key="item.id" :data="item"/>
  </List>
</template>

<script lang="ts">
import {defineComponent, nextTick, onBeforeUpdate, onUnmounted, onUpdated} from 'vue'
import Card from "./Card.vue";
import List from "./List.vue";

export default defineComponent({
  name: 'DemoNew',
  components: {
    Card,
    List
  },
  props:{
    data:{
      type:Array,
      default:()=>{}
    }
  },
  setup: () => {
    let counter = 0;
    let prevCounter = false;
    console.log('demo new setUp');
    onBeforeUpdate(()=>{
      prevCounter = !prevCounter;
      // console.log('new onBeforeUpdate', `${prevCounter}`);
      if(prevCounter===true){
        counter++;
        console.time(`new ${counter}`)
      }
    })
    onUpdated(()=>{
      nextTick(function () {
        console.timeEnd(`new ${counter}`);
      });
    })
    onUnmounted(()=>{
      console.log('demo new onUnmounted');
    })
    const getHeight = (item:{height:unknown}) => Number(item.height);
    return {getHeight}
  }
})
</script>