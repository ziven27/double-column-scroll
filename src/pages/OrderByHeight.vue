<template>
  <List :data="data" #="{ item, index }" :getHeight="(dataItem) => Number(dataItem.height)">
    <Card :data="item"/>
  </List>
</template>

<script lang="ts">
import {defineComponent} from 'vue'
import Card from "../components/Card.vue";
import List from "../components/List.vue";

export default defineComponent({
  name: 'OrderByHeight',
  components: {
    Card,
    List
  },
  props: {
    data: {
      type: Array,
      default: () => []
    }
  }
})
</script>