<template>
  <div class="wrapper">
    <h1>Thumbnail maker</h1>
    <div :style="style" class="main">
      <h1
        v-show="picked === 'title' || picked === 'both'"
        :style="fontStyle"
        class="title"
      >
        {{ title }}
      </h1>
      <font
        v-show="picked === 'desc' || picked === 'both'"
        :style="fontStyle"
        class="desc"
      >
        {{ desc }}
      </font>
    </div>
    <div>
      <input
        @change="changeBackground($event)"
        v-model="backgroundUrl"
        placeholder="이미지 url을 입력하세요"
      />
      <input v-model="title" placeholder="제목을 입력하세요" />
      <input v-model="desc" placeholder="설명을 입력하세요" />
      <form>
        <input type="radio" id="title" value="title" v-model="picked" />
        <label for="title">Title만</label>
        <input type="radio" id="desc" value="desc" v-model="picked" />
        <label for="desc">desc 만</label>
        <input type="radio" id="both" value="both" v-model="picked" />
        <label for="both">둘다</label>
      </form>
      <form>
        <input
          type="checkbox"
          id="fontSize"
          value="fontSize"
          v-model="FontSize"
          @change="onChangeFont"
        />
        <label for="fontSize">글자크게</label>
        <input
          type="checkbox"
          id="reversedColor"
          value="reversedColor"
          v-model="ReversedColor"
        />
        <label for="reversedColor">색상 반전</label>
      </form>
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
    </div>
    <ColorPicker
      theme="light"
      :color="color"
      :sucker-hide="true"
      :sucker-canvas="suckerCanvas"
      :sucker-area="suckerArea"
      @changeColor="changeColor"
      @openSucker="openSucker"
    />
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
  fontSize: boolean;
  reversedColor: boolean;
}

export default defineComponent({
  name: "Thumbnail",
  components: {
    ColorPicker,
  },
  data(): BaseComponent {
    return {
      backgroundUrl: "",
      title: " ",
      desc: " ",
      fontStyles: ["Do Hyeon", "Hahmlet", "Noto Sans KR", "Song Myung"],
      selected: "Do Hyeon",
      color: "#59c7f9",
      suckerCanvas: null,
      suckerArea: [],
      isSucking: false,
      picked: "title",
      fontSize: false,
      reversedColor: false,
    };
  },
  computed: {
    cssVars(): { "--font-size": string } {
      return {
        "--font-size": this.fontSize ? "2rem" : "1rem",
      };
    },
    style(): string {
      return this.backgroundUrl === null
        ? `background-color: ${this.color}`
        : `background-image: url(${this.backgroundUrl})`;
    },
    fontStyle(): string {
      return `font-family: ${this.selected}, serif`;
    },
  },
  methods: {
    changeColor(color: {
      rgba: { r: number; g: number; b: number; a: number };
    }) {
      const { r, g, b, a } = color.rgba;
      this.color = `rgba(${r}, ${g}, ${b}, ${a})`;
      this.backgroundUrl = null;
    },

    openSucker(isOpen: boolean) {
      if (isOpen) {
        // this.suckerCanvas = canvas;
        // this.suckerArea = [x1, y1, x2, y2];
      } else {
        // this.suckerCanvas && this.suckerCanvas.remove
      }
    },
  },
});
</script>

<style scss>
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.main {
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-direction: column;
  width: 768px;
  height: 402px;
  border: 1px solid black;
  background-size: cover;
}
.title {
  z-index: 100000;
  color: #000;
}
.desc {
  font-size: var(--font-size);
}
</style>
