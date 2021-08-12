<template>
  <div class="row">
    <div class="col">
      <template v-for="(item) in dataLeft" :key="item[keyName]">
        <slot :item="item"/>
      </template>
    </div>
    <div class="col">
      <template v-for="(item) in dataRight" :key="item[keyName]">
        <slot :item="item"/>
      </template>
    </div>
  </div>
</template>

<script lang="ts">
import {defineComponent, reactive, toRefs, watch} from 'vue'

// @ts-ignore
const getRenderData = ({data = [], getHeight}) => {
  const dataLeft: any = [];
  const dataRight: any = [];
  let anchorHeight = 0;

  // 默认单数往左，双数往右
  const defaultFilter = (item: { id: String }, key: number) => {
    if (key % 2 === 0) {
      console.log(`${item.id}, ${key}为双数，渲染到左边`);
      dataLeft.push(item);
    } else {
      console.log(`${item.id}, ${key}为单数，渲染到右边`);
      dataRight.push(item);
    }
  };

  // 基于用户返回的高度去决定渲染位置
  const anchorFilter = (item: { id: String }) => {
    const height = getHeight(item);
    if (anchorHeight > 0) {
      console.log(`${item.id}, ${anchorHeight} > 0, 渲染到右边, ${anchorHeight} - ${height} = ${anchorHeight - height}`);
      dataRight.push(item);
      anchorHeight = anchorHeight - height;
    } else {
      console.log(`${item.id}, ${anchorHeight} < = 0, 渲染到左边, ${anchorHeight} + ${height} = ${anchorHeight + height}`);
      dataLeft.push(item);
      anchorHeight = anchorHeight + height;
    }
  };

  data.forEach((item, key) =>
      typeof getHeight === 'function' ?
          anchorFilter(item) :
          defaultFilter(item, key)
  );

  return {dataLeft, dataRight}
}

export default defineComponent({
  name: 'List',
  props: {
    data: {
      type: Array,
      default: () => []
    },
    /* 防止重复渲染 */
    keyName: {
      type: String,
      required: true
      // default: 'id'
    },
    /* 获取高度 */
    getHeight: {
      type: Function,
      // default: (item) => Number(item.height)
    }
  },
  setup: (props) => {
    // @ts-ignore
    const state = reactive(getRenderData(props));
    watch(() => props.data, () => {
      // @ts-ignore
      const {dataLeft, dataRight} = getRenderData(props);
      state.dataLeft = dataLeft;
      state.dataRight = dataRight;
    })
    return {
      ...toRefs(state)
    }
  }
})
</script>

<style scoped>
.row {
  display: flex;
  flex-wrap: wrap;
}

.col {
  flex: 1;
  margin: 0 8px;
}
</style>
