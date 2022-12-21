<template>
    <div class="wrapper" @click="this.changeActiveItem(boardItem.flag)">
        <div class="card" :class="{ 'rotated': boardItem.status > 0 }">
            <div class="back">
                <div v-html="boardItem.svg"></div>
            </div>
            <div class="front">
                <span>?</span>
            </div>
        </div>
    </div>
</template>
<script>
import { inject } from 'vue'
export default {
    name: 'BoardItem',
    props: {
        boardItem: Object,
    },
    setup() {
        const changeActiveItem = inject('changeActiveItem');
        return {
            changeActiveItem
        }
    },
}
</script>




<style lang="less">
.wrapper {
    width: 100px;
    height: 100px;
    background-color: #fef6e4;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;

    .card {
        width: 100%;
        height: 100%;
        position: relative;
        perspective: 1000px;

        &.rotated {
            .back {
                transform: rotateY(360deg);
            }

            .front {
                transform: rotateY(180deg);
            }
        }


        .back {
            transform: rotateY(180deg);
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
            background-color: #fef6e4;
        }

        .front {
            img {
                max-width: 100%;
                min-width: 100%;
                height: auto;
            }
            span{
                font-size: 40px;
                font-weight: bold;
                color: #001858;
            }
            &:hover{
                background-color: #f3d2c1;
                span{
                    color: #fef6e4;
                }
            }
        }
    }
}
</style>