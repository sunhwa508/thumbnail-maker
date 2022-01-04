<template>
  <div :style="$emit('style')" class="main">
    <h1 :style="$emit('fontStyle')" class="title">
      {{ title }}
    </h1>
    <span :style="$emit('descfontStyle')" class="desc">
      {{ desc }}
    </span>
  </div>
</template>

<script lang="ts">
export default {
  components: {},
  props: ["title", "desc"],
  emits: ["style", "fontStyle", "descfontStyle", "spanStyle"],
};
</script>
<style scss>
* {
  margin: 0;
  padding: 0;
  font-family: "Do Hyeon", "serif";
}
.menu {
  display: flex;
}
.options {
  margin: 20px;
  background-color: #fff;
  border-radius: 5px;
}
.options select {
  margin: 20px;
  border: 1px solid #eaeaea;
  padding: 5px;
  border-radius: 5px;
}
body {
  background-image: url("https://i.pinimg.com/originals/3d/fc/45/3dfc454828df3b405411464c97bb98ec.png");
  background-repeat: no-repeat;
  background-position: center;
  background-size: 100% 100%;
}
.checkbox {
  text-align: center;
  font-size: 20px;
  margin: 20px;
}
.option-box {
  font-size: 20px;
  text-align: center;
  margin: 20px;
}
select {
  font-size: 1rem;
}
input {
  padding: 5px;
  border: 1px solid #eaeaea;
  border-radius: 5px;
  margin: 10px;
}
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.main {
  background-color: #fff;
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-direction: column;
  width: 768px;
  height: 402px;
  border: 2px solid #fff;
  background-size: cover;
}
.title {
  z-index: 100000;
  color: #000;
}
</style>
