<script setup>
import { onMounted } from "vue";
import lottie from "lottie-web";
import one from "./json/1.json";
import two from "./json/2.json";
import three from "./json/3.json";
import four from "./json/4.json";

let id = 1;
const clientHeight = window.innerHeight;
const animation = [
  {
    id: id++,
    ele: null,
    json: one,
    flag: true,
  },
  {
    id: id++,
    ele: null,
    json: two,
    flag: true,
  },
  {
    id: id++,
    ele: null,
    json: three,
    flag: true,
  },
  {
    id: id++,
    ele: null,
    json: four,
    flag: true,
  },
];

onMounted(() => {
  initLottie();
  onlyPlayOneLottie();

  window.addEventListener("scroll", judgeStartLottie);
});

function initLottie() {
  animation.forEach((item) => {
    item.ele = document.getElementById(`ref_${item.id}`);

    item.aniFn = createLottie({
      container: item.ele,
      animationData: item.json,
    });
  });
}

function onlyPlayOneLottie() {
  animation[0].flag = false;
  animation[0].aniFn.play();
}

function judgeStartLottie() {
  animation.forEach((item) => {
    item.offsetTop = item.ele.getBoundingClientRect().top;

    if (clientHeight >= item.offsetTop && item.flag) {
      item.flag = false;
      console.log(item);
      item["aniFn"].play();
    }
  });
}

function createLottie(obj) {
  const { container, animationData } = obj;

  return lottie.loadAnimation({
    container,
    renderer: "svg",
    autoplay: false,
    loop: false,
    animationData,
  });
}
</script>

<template>
  <div>
    <div
      v-for="ani in animation"
      :key="ani.id"
      :id="`ref_${ani.id}`"
      class="lottie-item"
    ></div>
  </div>
</template>

<style scoped>
.lottie-item {
  width: 1200px;
  height: 1200px;
  margin-top: 20px;
}
</style>
