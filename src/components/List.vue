<template>
  <div class="row">
    <div class="col">
      <template v-for="(item, index) in dataCols[0]">
        <slot
            :item="item"
            :index="index"
        />
      </template>
    </div>
    <div class="col">
      <template v-for="(item, index) in dataCols[1]">
        <slot
            :item="item"
            :index="index"
        />
      </template>
    </div>
  </div>
</template>

<script lang="ts">
import {
  defineComponent, PropType, reactive, toRef, watch
} from 'vue'


interface IState {
  dataCols: any[]
}

export default defineComponent({
  name: 'OnixDualColLayout',
  props: {
    /*数据源*/
    data: {
      type: Array as PropType<any[]>,
      default: () => []
    },

    /*获取子元素高度*/
    getItemHeight: {
      type: Function
    }
  },
  setup: (props: any) => {
    let anchorHeight = 0
    const state = reactive<IState>({
      dataCols: [[], []],
    })

    // 默认渲染方式
    const getDataColsDefault = (data: any[] = [], dataPrevLength:number = 0) => {
      const result: any[] = [[], []]
      data.forEach((item, key) => {
        const dataIndex = (key + dataPrevLength) % 2 === 0 ? 0 : 1
        // console.log(`${item.id}, ${key}为双数，渲染到左边`);
        result[dataIndex].push(item)
      })
      return result
    }

    // 自定义的方式
    const getDataColsCustom = (data: any[] = []) => {
      const result: any[] = [[], []]
      data.forEach(item => {
        const thisHeight = props.getItemHeight(item)
        if (anchorHeight > 0) {
          // console.log(`${item.id}, ${anchorHeight} > 0, 渲染到右边, ${anchorHeight} - ${height} = ${anchorHeight - height}`)
          result[1].push(item)
          anchorHeight -= thisHeight
        } else {
          // console.log(`${item.id}, ${anchorHeight} < = 0, 渲染到左边, ${anchorHeight} + ${height} = ${anchorHeight + height}`)
          result[0].push(item)
          anchorHeight += thisHeight
        }
      })
      return result
    }

    // 获取双列数据
    // 使用方定义了 getItemHeight 表示不使用默认的渲染方式
    const getDataCols = (data: any[], dataPrevLength: number) => (typeof props.getItemHeight === 'function'
        ? getDataColsCustom(data)
        : getDataColsDefault(data, dataPrevLength))

    // 首次渲染（全量）
    state.dataCols = getDataCols(props.data, 0)


    // data 发生变化重新获取 dataCols 的值
    watch(() => props.data, (dataCur: any[], dataPrev: any[]) => {
      const [curLeft, curRight] = state.dataCols

      // 之后渲染（增量）
      const [appendLeft, appendRight] = getDataCols(dataCur.slice(dataPrev.length), dataPrev.length)
      state.dataCols = [[...curLeft, ...appendLeft], [...curRight, ...appendRight]]
    })

    return {
      dataCols: toRef(state, 'dataCols'),
    }
  },
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
