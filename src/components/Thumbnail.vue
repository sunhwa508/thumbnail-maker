<template>
  <h1>Thumbnail maker</h1>
  <div class="wrapper">
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
        <span>폰트 설정</span>
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
        <br />
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

<style>
@import url("../../node_modules/vue-color-kit/dist/vue-color-kit.css");
</style>

<script lang="ts">
import { ColorPicker } from "vue-color-kit";
import { ref, reactive, ComputedRef, Ref, computed } from "vue";

declare interface BaseComponent {
  backgroundUrl?: Ref<string | null>;
  title?: Ref<string>;
  desc?: Ref<string>;
  fontStyles?: string[];
  selected?: Ref<string>;
  color?: Ref<string>;
  suckerCanvas?: Ref<null>;
  isSucking?: Ref<boolean>;
  picked?: Ref<string>;
  baseFontSize?: Ref<boolean>;
  reversedColor?: Ref<boolean>;
  hasShadow?: Ref<boolean>;
  onChangeFont: () => void;
  onChangeFontShadow: () => void;
  onChangeReversedColor: () => void;
  changeColor: (pickedColor: {
    rgba: { r: number; g: number; b: number; a: number };
  }) => void;
  style: ComputedRef<string>;
  fontStyle: ComputedRef<{
    fontFamily: string;
    fontSize: string;
    color: string;
    textShadow: string | null;
    opacity: number;
  }>;
  descfontStyle: ComputedRef<{
    fontFamily: string;
    fontSize: string;
    color: string;
    textShadow: string | null;
    opacity: number;
  }>;
  spanStyle: ComputedRef<{ color: string; backgroundColor: string }>;
}

export default {
  // name: "Thumbnail",
  components: {
    ColorPicker,
  },
  setup(): BaseComponent {
    let backgroundUrl = ref(null);
    let title = ref("");
    let desc = ref("");
    let fontStyles = reactive([
      "Do Hyeon",
      "Hahmlet",
      "Noto Sans KR",
      "Song Myung",
    ]);
    let selected = ref("Do Hyeon");
    let color = ref("#59c7f9");
    let picked = ref("both");
    let baseFontSize = ref(false);
    let reversedColor = ref(false);
    let hasShadow = ref(false);
    const onChangeFont = () => {
      baseFontSize.value = !baseFontSize.value;
    };
    const onChangeFontShadow = () => {
      hasShadow.value = !hasShadow.value;
    };
    const onChangeReversedColor = () => {
      reversedColor.value = !reversedColor.value;
    };
    const changeColor = (pickedColor: {
      rgba: { r: number; g: number; b: number; a: number };
    }) => {
      const { r, g, b, a } = pickedColor.rgba;
      color.value = `rgba(${r}, ${g}, ${b}, ${a})`;
      backgroundUrl.value = null;
    };

    const style = computed(() => {
      return backgroundUrl.value === null
        ? `background-color: ${color.value}`
        : `background-image: url(${backgroundUrl.value})`;
    });
    const fontStyle = computed(() => {
      return {
        fontFamily: `${selected.value}, serif`,
        fontSize: baseFontSize.value ? "4rem" : "2.5rem",
        color: reversedColor.value ? "#fff" : "#000",
        textShadow: hasShadow.value ? `2px 4px 3px rgba(0,0,0,0.3)` : null,
        opacity: picked.value === "title" || picked.value === "both" ? 1 : 0,
      };
    });
    const spanStyle = computed(() => {
      return { backgroundColor: "#333", color: "#ddd" };
    });
    const descfontStyle = computed(() => {
      return {
        fontFamily: `${selected.value}, serif`,
        fontSize: baseFontSize.value ? "2rem" : "1.2rem",
        color: reversedColor.value ? "#fff" : "#000",
        textShadow: hasShadow.value ? `2px 4px 3px rgba(0,0,0,0.3)` : null,
        opacity: picked.value === "desc" || picked.value === "both" ? 1 : 0,
      };
    });

    return {
      desc,
      title,
      picked,
      style,
      spanStyle,
      selected,
      fontStyles,
      fontStyle,
      descfontStyle,
      onChangeFont,
      onChangeFontShadow,
      onChangeReversedColor,
      changeColor,
    };
  },
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
