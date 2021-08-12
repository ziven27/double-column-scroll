<template>
  <div class="card" :style="{height: data.height+'px'}">
    {{ data.id }}
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, onUpdated, onUnmounted } from 'vue'
export default defineComponent({
  name: 'Card',
  props: {
    data:{
      type: Object,
      default: ()=>{}
    }
  },
  setup: (props) => {

    const text = props.data.id;
    onMounted(() => {
      console.log(text,'mounted!')
    })
    onUpdated(() => {
      console.log(text, 'updated!')
    })
    onUnmounted(() => {
      console.log(text,'unmounted!')
    })

    // const renderRun=(str)=> {
    //   // console.log('renderRun', str);
    //   return str;
    // };

    return {}
  }
})
</script>

<style scoped>
  .card{
    outline: 1px solid red;
    word-break: break-all;
    min-height: 100px;
    margin-bottom: 8px;
  }
</style>
