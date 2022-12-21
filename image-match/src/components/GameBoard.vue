<template>
  <div class="board">
    <div v-for="boardItem in boardItems" :key="boardItem.flag">
      <BoardItem :boardItem=boardItem>
      </BoardItem>
    </div>
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
      let flag = 0;
      for (let index = 0; index < 15; index++) {
        let boardObj = {
          status: 2,
          svg: toSvg(Math.random(), 40),
          pairNumber: index
        }
        boardItems.value.push({ ...boardObj, flag: flag });
        flag++;
        boardItems.value.push({ ...boardObj, flag: flag });
        flag++;
      }
      boardItems.value = shuffleArray(boardItems.value);
      setTimeout(() => {
        boardItems.value.map(item => item.status = 0);
      }, 4000)
    })
    const changeActiveItem = (flag) => {
      const activeItem = boardItems.value.find(item => item.flag === flag);

      if (activeItem.status === 2 || activeItem.status === 1) {
        return
      }

      if (boardItems.value.filter(item => item.status === 1).length > 0) {
        for (let i = 0; i < boardItems.value.length; i++) {
          if (boardItems.value[i].status === 1 && boardItems.value[i].flag != flag) {
            if (boardItems.value[i].pairNumber === activeItem.pairNumber) {
              boardItems.value[i].status = 2;
              activeItem.status = 2;
            }
            else {
              activeItem.status = 1;
              setTimeout(() => {
                boardItems.value[i].status = 0;
                activeItem.status = 0;
              }, 800)
            }
          }

        }
      }
      else {
        activeItem.status = 1;
      }
      checkWin();
    }

    const checkWin = () => {
      if (!boardItems.value.find(item => item.status === 0 || item.status === 1)) {
        window.alert('You win');
      }
    }

    const shuffleArray = (array) => {
      var currentIndex = array.length, temporaryValue, randomIndex;

      while (0 !== currentIndex) {

        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;

        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }

      return array;
    }

    provide('changeActiveItem', changeActiveItem);

    return {
      boardItems
    }
  },
  components: {
    BoardItem
  },
}
</script>


<style lang="less">
body {
  background-color: #fef6e4;
}

.board {
  position: absolute;
  margin-left: auto;
  margin-right: auto;
  left: 0;
  right: 0;
  display: flex;
  width: 600px;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}
</style>