<template>
    <div class="carousel">
        <slot :currentSlide="currentSlide"/>

        <!-- Navigation -->
        <div class="navigate">
            <div class="toggle-page left">
                <i @click="prevSlide"  class="fa-solid fa-chevron-left"></i>
            </div>
            <div class="toggle-page right">
                <i @click="nextSlide" class="fa-solid fa-chevron-right"></i>
            </div>
        </div>
        <!-- Pagination -->
        <div class="pagination">
            <span @click="setImage(index)" v-for="(slide, index) in getSlideCount" :key="index" :class="{active: index + 1 === currentSlide}"></span>
            <div @click="handleAutoplay" class="autoplay">
                <i v-if="!autoPlayEnabled" class="fa-solid fa-play"></i>
                <i v-if="autoPlayEnabled" class="fa-solid fa-pause"></i>
            </div>
        </div>

    </div>
</template>

<script>
import { ref, onMounted, onUnmounted }  from 'vue'
    export default {
        name: 'Carousel',
        setup(){
            const currentSlide = ref(1)
            const getSlideCount = ref(null)
            const autoPlayEnabled = ref(false);
            const timeoutDuration = ref(4000);
            const intervalId = ref(null)

            // slider
            const nextSlide = () => {
                if (currentSlide.value === getSlideCount.value) currentSlide.value = 1 
                else currentSlide.value += 1;
            }
            const prevSlide = () => {
                if (currentSlide.value === 1) currentSlide.value = getSlideCount.value 
                else currentSlide.value -= 1;
            }
            
            const setImage = (index) => {
                currentSlide.value = index + 1;
            }
            
            // Autoplay

            const autoplay = () => {
               intervalId.value = setInterval(() => {
                    nextSlide()
                }, timeoutDuration.value) 
            };
            if (autoPlayEnabled.value){
                autoplay();
            } 
            if(!autoPlayEnabled.value){
                clearInterval(intervalId.value);
            }
            

            const handleAutoplay = () => {
                autoPlayEnabled.value = !autoPlayEnabled.value;
                 if (autoPlayEnabled.value) autoplay();
                 else if (!autoPlayEnabled.value) clearInterval(intervalId.value);
            }

            onMounted(() => {
                getSlideCount.value = document.querySelectorAll('.slide').length;
                handleAutoplay();
            })

            onUnmounted(() => {
                clearInterval(intervalId.value)
            })

            return {
                currentSlide,
                nextSlide,
                prevSlide,
                getSlideCount,
                setImage,
                handleAutoplay,
                autoPlayEnabled
            }
        }
    }
</script>

<style scoped>

.navigate{
    padding: 0 16px;
    height: 100%;
    width: 100%;
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
}

.toggle-page{
    display: flex;
    flex : 1;
}

.right {
    justify-content: flex-end;
}

.navigate i{
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    width: 50px;
    height: 50px;
    background-color: #d6d6d6cc;
    color: rgb(0, 0, 0);
    font-size: 22px;
}

.pagination {
    position: absolute;
    bottom: 24px;
    width: 100%;
    gap: 16px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.pagination span {
    height: 27px;
    width: 27px;
    border-radius: 50%;
    border: 2px solid #fff;
    cursor: pointer;
}
.active {
    background: #e4e4e4d0;
}

.autoplay {
    /* position: absolute; */
    margin-left: 20px;
    cursor: pointer;
}
.autoplay i {
    display: flex;
    justify-content: center;
    align-items: center;
    color: white;
    height: 27px;
    width: 27px;
    font-size: 30px;
}
</style>