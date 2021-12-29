<template>
  <div class="wrapper">
    <h1>Thumbnail maker</h1>
    <div :style="style" class="main">
      <h1 :style="fontStyle" class="title">
        {{ title }}
      </h1>
      <span :style="descfontStyle" class="desc">
        {{ desc }}
      </span>
    </div>
    <div class="menu">
      <div class="options">
        <select v-model="selected">
          <option disabled value="">font를 선택하세요</option>
          <option
            v-for="(option, i) in fontStyles"
            v-bind:value="option"
            :key="i"
          >
            {{ option }}
          </option>
        </select>
        <input v-model="title" placeholder="제목을 입력하세요" />
        <input v-model="desc" placeholder="설명을 입력하세요" />
        <div class="option-box">
          <input type="radio" id="title" value="title" v-model="picked" />
          <label for="title">only Title</label>
          <input type="radio" id="desc" value="desc" v-model="picked" />
          <label for="desc">only Desc</label>
          <input type="radio" id="both" value="both" v-model="picked" />
          <label for="both">Both</label>
        </div>
        <div class="checkbox">
          <input
            type="checkbox"
            id="baseFontSize"
            value="baseFontSize"
            v-model="baseFontSize"
            @click="onChangeFont(baseFontSize)"
          />
          <label for="baseFontSize"><span>글자크게</span></label>
          <input
            type="checkbox"
            id="reversedColor"
            value="reversedColor"
            v-model="ReversedColor"
            @click="onChangeReversedColor(ReversedColor)"
          />
          <label for="reversedColor">색상 반전</label>
          <input
            type="checkbox"
            id="hasShadow"
            value="hasShadow"
            v-model="hasShadow"
            @click="onChangeFontShadow(hasShadow)"
          />
          <label for="hasShadow">그림자효과</label>
        </div>
      </div>
      <div class="options">
        <ColorPicker
          theme="light"
          :color="color"
          :sucker-hide="true"
          :sucker-canvas="suckerCanvas"
          :sucker-area="suckerArea"
          @changeColor="changeColor"
        />
        <input
          @change="changeBackground($event)"
          v-model="backgroundUrl"
          placeholder="이미지 url을 입력하세요"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { ColorPicker } from "vue-color-kit";
import "vue-color-kit/dist/vue-color-kit.css";

declare interface BaseComponent {
  backgroundUrl: string | null;
  title: string;
  desc: string;
  fontStyles: Array<string>;
  selected: string;
  color: string;
  suckerCanvas: null;
  suckerArea: [];
  isSucking: boolean;
  picked: string;
  baseFontSize: boolean;
  reversedColor: boolean;
  hasShadow: boolean;
}

export default defineComponent({
  name: "Thumbnail",
  components: {
    ColorPicker,
  },
  data(): BaseComponent {
    return {
      backgroundUrl: "",
      title: "",
      desc: "",
      fontStyles: ["Do Hyeon", "Hahmlet", "Noto Sans KR", "Song Myung"],
      selected: "Do Hyeon",
      color: "#59c7f9",
      suckerCanvas: null,
      suckerArea: [],
      isSucking: false,
      picked: "both",
      baseFontSize: false,
      reversedColor: false,
      hasShadow: false,
    };
  },
  computed: {
    style(): string {
      return this.backgroundUrl === null
        ? `background-color: ${this.color}`
        : `background-image: url(${this.backgroundUrl})`;
    },
    fontStyle(): {
      fontFamily: string;
      fontSize: string;
      color: string;
      textShadow: string | null;
      opacity: number;
    } {
      return {
        fontFamily: `${this.selected}, serif`,
        fontSize: this.baseFontSize ? "4rem" : "2.5rem",
        color: this.reversedColor ? "#fff" : "#000",
        textShadow: this.hasShadow ? `2px 4px 3px rgba(0,0,0,0.3)` : null,
        opacity: this.picked === "title" || this.picked === "both" ? 1 : 0,
      };
    },
    spanStyle(): {
      color: string;
      backgroundColor: string;
    } {
      return { backgroundColor: "#333", color: "#ddd" };
    },
    descfontStyle(): {
      fontFamily: string;
      fontSize: string;
      color: string;
      textShadow: string | null;
      opacity: number;
    } {
      return {
        fontFamily: `${this.selected}, serif`,
        fontSize: this.baseFontSize ? "2rem" : "1.2rem",
        color: this.reversedColor ? "#fff" : "#000",
        textShadow: this.hasShadow ? `2px 4px 3px rgba(0,0,0,0.3)` : null,
        opacity: this.picked === "desc" || this.picked === "both" ? 1 : 0,
      };
    },
  },
  methods: {
    onChangeFont() {
      this.baseFontSize = !this.baseFontSize;
    },
    onChangeFontShadow() {
      this.hasShadow = !this.hasShadow;
    },
    onChangeReversedColor() {
      this.reversedColor = !this.reversedColor;
    },
    changeColor(color: {
      rgba: { r: number; g: number; b: number; a: number };
    }) {
      const { r, g, b, a } = color.rgba;
      this.color = `rgba(${r}, ${g}, ${b}, ${a})`;
      this.backgroundUrl = null;
    },
  },
});
</script>
<style scss>
* {
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
body {
  background-image: url("https://i.pinimg.com/originals/3d/fc/45/3dfc454828df3b405411464c97bb98ec.png");
  background-repeat: no-repeat;
  background-position: center;
  background-size: 100% 100%;
}
.checkbox {
  width: 100px;
  text-align: center;
  font-size: 20px;
}
.option-box {
  font-size: 20px;
  width: 100px;
  text-align: center;
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
