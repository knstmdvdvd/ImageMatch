<template>
  <GameBoard :key="reloadGameKey"/>
</template>

<script>
import GameBoard from './components/GameBoard.vue'
import { ref,provide } from 'vue'

export default {
  name: 'App',
  components: {
    GameBoard
  },
  setup() {
    const reloadGameKey = ref(0)
    const reloadGame = () => {
      reloadGameKey.value++;
    }
    provide('reloadGame', reloadGame);
    return{
      reloadGameKey
    }
  }
}
</script>
