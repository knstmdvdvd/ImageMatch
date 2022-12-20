<template>
    <div class="wrapper" @click="this.changeActiveItem(index)">
        <div class="card" :class="{ 'rotated': boardItems[index].status > 0 }">
            <div class="back">
                <div v-html="boardItems[index].svg"></div>
            </div>
            <div class="front">
                <span>{{ index+1 }}</span>
            </div>
        </div>
    </div>
</template>
<script>
import {inject} from 'vue'
export default {
    name: 'BoardItem',
    props: {
        index: Number,
    },
    setup() {
        const changeActiveItem = inject('changeActiveItem');
        const boardItems = inject('boardItems');
        return{
            changeActiveItem,
            boardItems
        }
    },
}
</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.wrapper {
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
    background-color: #d5d5d5;
    display: flex;
    justify-content: center;
    align-items: center;
}

.card {
    width: 300px;
    height: 300px;
    position: relative;
    perspective: 1000px;
    border: 1px solid #fff;
}

.front,
.back {
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    transition: 1s;
    backface-visibility: hidden;
    background-color: #fff;
}

.front img {
    max-width: 100%;
    min-width: 100%;
    height: auto;
}

.back {
    transform: rotateY(180deg);
}

.card.rotated .front {
    transform: rotateY(180deg);
}

.card.rotated .back {
    transform: rotateY(360deg);
}
</style>