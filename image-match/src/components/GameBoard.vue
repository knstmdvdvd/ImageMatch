<template>
  <div v-for="boardItem in boardItems" :key="index">
    <BoardItem :index=index>
    </BoardItem>
  </div>
</template>

<script>
import { toSvg } from "jdenticon";
import BoardItem from './BoardItem.vue'
import { ref, onMounted, provide } from 'vue'

export default {
  name: 'GameBoard',
  setup() {
    const boardItems = ref([])
    onMounted(() => {
      for (let index = 0; index < 15; index++) {
        let boardObj = {
          status: 0,
          svg: toSvg(Math.random(), 100),
          pairNumber: index
        }
        boardItems.value.push(boardObj);
        boardItems.value.push(Object.assign({},boardObj));
      }
    })
    const changeActiveItem = (index) => {
      console.log(boardItems.value);
      if(boardItems.value[index].status === 2 || boardItems.value[index].status === 1){
        return
      }
      
      if(boardItems.value.filter(item => item.status === 1).length > 0){
        for (let i = 0; i < boardItems.value.length; i++) {
          if(boardItems.value[i].status === 1 && i != index){
            if(boardItems.value[i].pairNumber === boardItems.value[index].pairNumber){
              boardItems.value[i].status = 2;
              boardItems.value[index].status = 2;
            }
            else{
              boardItems.value[i].status = 1;
              boardItems.value[i].status = 0;
              boardItems.value[index].status = 0;
            }
          }
          
        }
      }
      else{
         boardItems.value[index].status = 1;
      }
    }

    provide('changeActiveItem',changeActiveItem);
    provide('boardItems',boardItems);

    return {
      boardItems
    }
  },
  components: {
    BoardItem
  },
  methods: {
  },
}
</script>
