<template>
  <div class="wrapper">
    <h1>Thumbnail maker</h1>
    <div :style="style" class="main"></div>
    <input v-model="backgroundUrl" placeholder="이미지 url을 입력하세요" />
    <input v-model="title" placeholder="제목을 입력하세요" />
    <h1 class="title">{{ title }}</h1>
    <p>메시지: {{ backgroundUrl }}</p>
  </div>
</template>

<script lang="ts">
declare interface BaseComponent {
  backgroundUrl: string;
  title: string;
}
export default {
  name: "Thumbnail",
  data(): BaseComponent {
    return {
      backgroundUrl: "",
      title: "",
    };
  },
  computed: {
    style(): string {
      return `background-image: url(${this.backgroundUrl})`;
    },
  },
};
</script>

<style scss>
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.main {
  position: relative;
  width: 768px;
  height: 402px;
  border: 1px solid black;
}
.title {
  z-index: 100000;
  position: absolute;
  top: 30;
  color: #fff;
}
</style>
