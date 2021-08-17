<template>
  <div class="header-wrap">
    <div class="header">
      <div class="df aic jcsb mb8">
        <select @change="changeOrderType">
          <option value="height">按高度排序 (优化后)</option>
          <option value="key">按索引排序 (优化前)</option>
        </select>
        <p v-if="tabType===TAB_TYPE.HEIGHT">按照每个子模块高度排序</p>
        <p v-if="tabType===TAB_TYPE.KEY">单数往左双数往右</p>
      </div>
      <div class="df aic">
        每次添加：
        <input type="number" v-model="appendNumber">
        <button type="button" @click="appendData">添加数据</button>
      </div>
    </div>
  </div>
  <demo-new v-if="tabType===TAB_TYPE.HEIGHT" :data="data" />
  <demo-old v-if="tabType===TAB_TYPE.KEY" :data="data" />
</template>

<script lang="ts">
import {defineComponent, nextTick, reactive, toRefs} from 'vue'
import DemoOld from "./components/DemoOld.vue";
import DemoNew from "./components/DemoNew.vue";

// 创建随机的数据
const getRandomData = (length = 10) => {
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

type TYPE_STATE = {
  data: any[]
  tabType:TAB_TYPE
  appendNumber: number
}

export default defineComponent({
  name: 'App',
  components: {
    DemoOld,
    DemoNew
  },
  setup: () => {
    const state = reactive<TYPE_STATE>({
      data: [],
      tabType: TAB_TYPE.HEIGHT,
      appendNumber: 10
    })

    // 添加随机数据
    const appendData = () => {
      state.data = [...state.data, ...getRandomData(state.appendNumber)];
      nextTick(() => {
        // 滚动到底部
        window.scrollTo(0, document.documentElement.scrollHeight);
      })
    }

    // 修改类型
    // @ts-ignore
    const changeOrderType = (e) => {
      state.tabType = e.target.value;
      state.data = getRandomData(state.appendNumber);
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

.df{
  display: flex;
}
.aic{
  align-items: center;
}
.jcsb{
  justify-content: space-between;
}
.mb8{
  margin-bottom: 8px;
}
</style>
<style scoped>
.header-wrap {
  height: 80px;
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
}

.header p {
  margin: 0;
}

.header select {
  margin-right: 8px;
}

.header button {
  margin-left: auto;
}
</style>
