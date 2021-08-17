<script>
import {defineComponent, h} from "vue";

export default defineComponent({
  name: 'CubeDualColumnLayout',
  setup(props, {slots}) {
    return () => {
      const [{children}] = slots.default();
      return h(
          'div',
          {class: 'dual-column-layout old'},
          [
            h(
                'div',
                {class: `dual-column-layout--column --left`},
                children.filter((_, i) => i % 2 === 0)
            ),
            h(
                'div',
                {class: 'dual-column-layout--column --right'},
                children.filter((_, i) => i % 2 === 1)
            ),
          ]
      )
    }
  },
})
</script>

<style scoped>
.dual-column-layout {
  padding: 0px 8px;
  display: flex;
}

.dual-column-layout--column {
  flex: 1;
  min-width: 0;
}

.dual-column-layout--column > * {
  margin-top: 5px;
  overflow: hidden;
}
.dual-column-layout--column + .dual-column-layout--column{
  margin-left: 16px;
}
</style>
