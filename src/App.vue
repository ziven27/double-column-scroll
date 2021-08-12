<template>
  <div class="header-wrap">
    <div class="header">
      <select @change="changeOrderType">
        <option value="height">按高度排序</option>
        <option value="key">按索引排序</option>
      </select>
      <p v-if="tabType===TAB_TYPE.HEIGHT">按照每个子模块高度动态计算</p>
      <p v-if="tabType===TAB_TYPE.KEY">单数往左双数往右</p>
      <button type="button" @click="appendData">添加数据</button>
    </div>
  </div>
  <List v-if="tabType===TAB_TYPE.HEIGHT" :data="data" #="{ item, index }"
        :getHeight="dataItem => Number(dataItem.height)" keyName="id">
    <Card :data="item"/>
  </List>
  <!-- 区别就在于是否定义了 getHeight 这个方法 -->
  <List v-if="tabType===TAB_TYPE.KEY" :data="data" #="{ item, index }" keyName="id">
    <Card :data="item"/>
  </List>
</template>

<script lang="ts">
import {defineComponent, nextTick, reactive, toRefs} from 'vue'
import Card from "./components/Card.vue";
import List from "./components/List.vue";

// 创建随机的数据
const getRandomData = (length = 3) => {
  const result = [];
  for (let i = 0; i < length; i++) {
    result.push({
      id: Number(Math.random().toString().substr(2, 2) + Date.now()).toString(36),
      height: Math.floor(Math.random() * 200 + 100)
    })
  }
  return result;
};

enum TAB_TYPE {
  HEIGHT = 'height',
  KEY = 'key'
}

export default defineComponent({
  name: 'App',
  components: {
    Card,
    List
  },
  setup: () => {
    const state = reactive({
      data: getRandomData(),
      tabType: TAB_TYPE.HEIGHT
    })

    // 添加随机数据
    const appendData = () => {
      state.data = [...state.data, ...getRandomData()];
      nextTick(() => {
        // 滚动到底部
        window.scrollTo(0, document.documentElement.scrollHeight);
      })
    }

    // 修改类型
    const changeOrderType = (e) => {
      state.tabType = e.target.value;
      state.data = getRandomData();
    }

    return {...toRefs(state), appendData, TAB_TYPE, changeOrderType}
  }
})
</script>
<style>

html, body {
  /* 平滑滚动 */
  scroll-behavior: smooth;
}
</style>
<style scoped>
.header-wrap {
  height: 50px;
}

.header {
  font-size: 12px;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  padding: 8px;
  background-color: #ffffff;
  border-bottom: 1px solid #ccc;
  display: flex;
  align-items: center;
}
.header p{
  margin: 0;
}
.header select {
  margin-right: 8px;
}

.header button {
  margin-left: auto;
}
</style>
