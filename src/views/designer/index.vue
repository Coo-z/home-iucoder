<template>
  <div ref="el" class="icon-scroll-content">
    <template v-for="item in list">
      <div :key="item.id" v-if="item.type == 'icon' || item.type == 'add'"
        class="icon-box flex flex-direction justify-center align-center">
        <div style=" height: 70%;" :title='item.name'>
          <img class="app-item-img" :src="item.src" style=" height: 100%; border-radius: 15px;" />
        </div>
        <p style='flex:1;filter: drop-shadow(0px 2px 7px rgba(0,0,0,.1));margin-top: 5px;text-shadow: 0 0 2px #0000004d;'
          class="cl-ant-p sg-omit-sm text-white-sm">{{ item.name }}
        </p>
      </div>
      <div v-else-if="item.type == 'component'" :key="item.component"
        class="comp-box flex flex-direction justify-center align-center">
        <component :is="HeadCalendar" />
      </div>
    </template>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useDraggable, type UseDraggableReturn } from 'vue-draggable-plus'
import { useGridsStore } from '@/store/grids';
import HeadCalendar from '../home/HeadCalendar.vue';
const drag = ref(false)


const el = ref()
const list = ref([])
onMounted(() => {
  list.value = useGridsStore().getSelectedGrids.navIconConfig
})

// 返回值是一个对象，包含了一些方法，比如 start、destroy、pause 等
const draggable = useDraggable<UseDraggableReturn>(el, list, {
  animation: 150,
  onStart() {
    console.log('start')
    drag.value = true
  },
  onEnd() {
    console.log('onEnd')
    nextTick(() => {
      drag.value = false
    })
  },
  onUpdate() {
    console.log('update')
  }
})

const sort = () => {
  list.value.sort((a, b) => a.id - b.id)
}
</script>

<style scoped lang="scss">
.icon-scroll-content {
  display: grid;
  /* 这个属性用于定义网格布局中的列。在这个例子中，repeat(auto-fill, 200px)表示列的宽度为200像素，并且会自动填充容器的宽度，以适应容器的大小。 */
  grid-template-columns: repeat(auto-fill, 100px);
  /* 控制自动布局算法的流动方式。dense表示使用“密集”布局算法*/
  grid-auto-flow: row dense;
  /* 行列间距。 */
  grid-gap: 10px;
  justify-content: center;
  grid-auto-rows: auto;
  width: 80%;

  .icon-box {
    width: 100%;
    height: 95px;
    grid-column: span 1;
    grid-row: span 1;
    cursor: pointer;
    padding: 10px 0px;
  }

  .comp-box {
    grid-column: span 2;
    grid-row: span 2;
    grid-column-start: span 2;
    grid-row-start: span 2;
    height: 200px;
  }
}
</style>

<style>
.fade-move,
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: scaleY(0.01) translate(30px, 0);
}

.fade-leave-active {
  position: absolute;
}
</style>