<template>
    <div class="img-carousel">
        <div class="img-container">
            <transition-group tag="div" class="img-slider" :name="transition">
                    <img v-show="index==imgIndex" v-for="(img, index) in imgArray" :key="index" :src="img"/>
            </transition-group>
            <div class="arrows">
                <div @click="prev" class="left-arrow">
                    <span class="material-symbols-outlined">chevron_left</span>
                </div>
                <div @click="next" class="right-arrow">
                    <span class="material-symbols-outlined">chevron_right</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default{
        name: "ImgCarousel",
        data() {
            return {
                imgIndex:0,
                transition: "slide-left"
            }
        },
        props: {
          imgArray: {
            type: Array,
            required: true
          }  
        },
        methods: {
            increment () {
                this.imgIndex++
                if (this.imgIndex==this.imgArray.length) {
                    this.imgIndex=0
                }
            },
            galleryRotate () {
                setInterval(this.increment, 2000)
            },
            next () {
                this.transition = "slide-left"
                this.increment()
            },
            prev () {
                this.transition = "slide-right"
                this.imgIndex--
                if (this.imgIndex<0) {
                    this.imgIndex=this.imgArray.length-1
                }
            }
        },
        mounted () {
            // this.galleryRotate()
        }
    }
</script>

<style lang="scss">
    .img-carousel {
        width: 500px;
        height: 500px;
        margin: 0 auto;
        .img-container {
            width: 100%;
            height: 100%;
        }
        img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
    }
    .arrows {
        display: flex;
        justify-content: space-between;
        span {
            cursor: pointer;
        }
    }
    .slide-left-leave-active, .slide-left-enter-active {
        transition: all 1s;
    }
    .slide-left-enter-from {
        transform: translateX(100%);
    }
    .slide-left-leave-to {
        transform: translateX(-100%);
    }
    .slide-right-leave-active, .slide-right-enter-active {
        transition: all 1s;
    }
    .slide-right-enter-from {
        transform: translateX(-100%);
    }
    .slide-right-leave-to {
        transform: translateX(100%);
    }
    .img-slider {
        position: relative;
        width: 500px;
        height: 500px;
        overflow: hidden;
        img {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
        }
    }
</style>