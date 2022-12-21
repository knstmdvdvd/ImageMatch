<template>
  <div class="board">
    <div v-for="boardItem in boardItems" :key="boardItem.flag">
      <BoardItem :boardItem=boardItem>
      </BoardItem>
    </div>
    <div v-if="win" class="overlay">
      <span class="text">
        Вы выиграли!
      </span>
      <div class="reload" @click="reload()">
        <svg xmlns="http://www.w3.org/2000/svg" version="1.0" width="64.000000pt" height="64.000000pt"
          viewBox="0 0 64.000000 64.000000" preserveAspectRatio="xMidYMid meet">

          <g transform="translate(0.000000,64.000000) scale(0.100000,-0.100000)" fill="#000000" stroke="none">
            <path
              d="M517 614 c-4 -4 -7 -25 -7 -46 0 -21 -2 -38 -5 -38 -2 0 -20 11 -40 24 -44 30 -134 50 -184 41 -107 -19 -196 -97 -226 -198 -19 -65 -19 -86 1 -94 20 -8 34 13 34 52 0 17 15 55 33 85 63 106 195 141 307 83 29 -16 52 -32 51 -38 -1 -5 -18 -16 -39 -23 -20 -7 -43 -19 -51 -27 -27 -27 17 -30 91 -6 l69 22 -3 81 c-3 75 -13 100 -31 82z" />
            <path
              d="M556 324 c-3 -9 -6 -28 -6 -43 0 -15 -15 -51 -32 -81 -64 -106 -196 -141 -308 -82 -29 15 -53 31 -54 36 0 5 17 15 39 23 105 36 70 68 -37 34 l-69 -22 3 -82 c2 -65 6 -82 18 -82 10 0 16 13 18 43 2 23 5 42 7 42 2 0 20 -11 40 -24 116 -79 294 -41 374 81 30 45 58 144 46 163 -9 15 -32 12 -39 -6z" />
          </g>
        </svg>
      </div>
    </div>
  </div>
</template>

<script>
import { toSvg } from "jdenticon";
import BoardItem from './BoardItem.vue'
import { ref, onMounted, provide,inject } from 'vue'

export default {
  name: 'GameBoard',
  setup() {
    const boardItems = ref([])
    const win = ref(false);
    const reloadGame = inject('reloadGame');
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
        win.value = true;
      }
    }

    const reload = () => {
      reloadGame();
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
      boardItems,
      win,
      reload
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

.flex-center {
  align-items: center;
  justify-content: center;
  display: flex;
}

.board {
  position: absolute;
  margin-left: auto;
  margin-right: auto;
  left: 0;
  right: 0;
  width: 600px;
  flex-wrap: wrap;
  .flex-center();


  .overlay {
    position: fixed;
    width: 100vw;
    height: 100vh;
    .flex-center();
    flex-direction: column;
    background-color: #bae8e8;
    opacity: 0.8;
    top: 0;
    z-index: 10;

    .text {
      font-size: 100px;
      color: #272343;
      font-weight: bold;
    }

    .reload {
      cursor: pointer;
    }
  }
}
</style>