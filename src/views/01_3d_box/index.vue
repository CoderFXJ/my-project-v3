<template>
  <div class="wrapper">
    <div class="opt-wrap">
      <button class="btn" @click="handleTransform">CLICK🎲</button>
    </div>
    <div
      class="img-wrap"
      :class="{
        rotate: isRotated,
      }"
      :style="imgStyle"
    >
      <div class="row" v-for="(rowArr, index) in styleArr" :key="index">
        <div
          class="col"
          v-for="(item, idx) in rowArr"
          :key="idx"
          :style="item"
        ></div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, reactive, computed, watch, defineProps } from "vue";
import imageSrc from '../../assets/images/01.png'
console.log('imageSrc: ',  imageSrc);

const { rowCount, colCount,src } = defineProps({
  rowCount: {
    type: Number,
    default: 4,
  },
  colCount: {
    type: Number,
    default: 4,
  },
  src:{
    type:String,
    default: imageSrc
  }
});
const image = new Image();
image.src = src
//输出图片宽高
const imgInfo = reactive({
  width: 0,
  height: 0,
});
//图片加载完毕
image.onload = function () {
  imgInfo.width = image.width || 640;
  imgInfo.height = image.height || 640;
  console.log('imgInfo: ', imgInfo);
};
//转换样式
const imgStyle = computed(() => {
  let { width, height } = imgInfo;
  return {
    width: `${width}px`,
    height: `${height}px`,
  };
});
interface itemStyle {
  height: string;
  width: string;
  backgroundImage: string;
  backgroundPosition: string;
}
let styleArr = reactive<itemStyle[][]>([]);
const initImage = () => {
  styleArr = reactive([]);
  let iheight = imgInfo.height / rowCount;
  console.log('iheight: ', iheight);
  let iwidth = imgInfo.width / colCount;
  console.log('iwidth: ', iwidth);

  //生成各个方块的样式及背景图坐标
  for (let i = 0; i < rowCount; i++) {
    let rowArr = [];
    for (let j = 0; j < colCount; j++) {
      rowArr.push({
        height: `${iheight}px`,
        width: `${iwidth}px`,
        backgroundImage: `url(${src})`,
        backgroundPosition: `${-j * iwidth}px ${-i * iheight}px`,
        backgroundSize: `${imgInfo.width}px ${imgInfo.height}px`,//必须更新
      });
    }
    styleArr.push(rowArr);
    console.log('styleArr: ', styleArr);
  }
};

watch( imgInfo, () => { initImage() });

const isRotated = ref(false);//是否以旋转
const handleTransform = () => {
  imgInfo.height = imgInfo.height + (isRotated.value ? -100 : 100);
  imgInfo.width = imgInfo.width + (isRotated.value ? -100 : 100);
  isRotated.value = !isRotated.value;
};
</script>
<style scoped lang="scss">
.wrapper {
  text-align: center;
  .opt-wrap {
    margin: 20px auto;
    justify-content: center;
    .btn {
      outline: none;
      border: none;
      padding: 4px 6px;
      box-shadow: 0 0 3px #ddd;
      border-radius: 3px;
      font-size: 16px;
      cursor: pointer;
      &:hover {
        box-shadow: 2px 2px 4px #ccc;
      }
    }
  }
  .img-wrap {
    margin: auto;
    .row {
      display: flex;
      align-items: center;
      justify-content: center;
      .col {
        flex: 1;
        background-repeat: no-repeat;
        transition: 0.4s ease;
        position: relative;
        &::after {
          content: "";
          background-color: #f6e58d;
          position: absolute;
          top: 8px;
          right: -15px;
          height: 100%;
          width: 15px;
          transform: skewY(45deg);
        }
        &::before {
          content: "";
          background-color: #f9ca24;
          position: absolute;
          bottom: -15px;
          left: 8px;
          height: 15px;
          width: 100%;
          transform: skewX(45deg);
        }
      }
    }
    &.rotate .row .col {
      transform: rotateZ(360deg);
    }
  }
}
</style>
