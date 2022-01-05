<template>
  <div :style="divStyle" class="main">
    <h1 :style="fontStyle" class="title">
      {{ title }}
    </h1>
    <span :style="descfontStyle">
      {{ desc }}
    </span>
  </div>
</template>

<script>
export default {
  props: ["title", "desc", "divStyle", "fontStyle", "descfontStyle"],
};
</script>
<style scss>
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
