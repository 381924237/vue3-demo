<script setup lang="ts">
import { ref } from 'vue';
import slideList from './slides/slideList'

const slideIndex = ref(0);

const changeSlide = (i: number) => {
  slideIndex.value = i;
}

const next = () => {
  if (slideIndex.value + 1 >= slideList.length) {
    slideIndex.value = 0
  } else {
    slideIndex.value++;
  }

}

const pre = () => {
  if (slideIndex.value === 0) {
    slideIndex.value = slideList.length - 1
  } else {
    slideIndex.value--;
  }
}

</script>

<template>
  <div class="container">
    <div class="nav">
      <div @click="changeSlide(index)" v-for="(item, index) in slideList" :key="index" class="nav-item" :class="{
        active: index === slideIndex
      }">
        {{ item.title }}
      </div>
    </div>
    <div class="content-wrap">
      <div class="content">
        <Transition v-for="(item, index) in slideList" name="slide" :key="index">
          <div class="slide-item" v-if="index === slideIndex">
            <component :is="item.component"></component>
          </div>
        </Transition>
      </div>
      <div class="control">
        <span class="control-btn" @click="pre">pre</span>
        <span class="control-btn" @click="next">next</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  height: 100vh;
  width: 100%;
  display: flex;
}

.content-wrap,
.content {
  flex: 1;
}

.content-wrap {
  display: flex;
  flex-direction: column;
}

.control {
  display: flex;
  justify-content: center;
  align-items: center;
}

.control-btn {
  width: 100px;
  padding: 10px;
  margin: 0 20px;
  cursor: pointer;
}

.nav {
  min-width: 250px;
  padding: 25px 0;
}

.nav-item {
  cursor: pointer;
  padding: 10px;
}

.active {
  background: #eee;
}

.slide-item {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}


.slide-enter-active {
  transition: opacity 0.8s ease;
}

.slide-enter-from,
.slide-leave-to {
  opacity: 0;
}
</style>
