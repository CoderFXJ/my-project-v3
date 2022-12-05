<template>
  <div class="img-wrap" :style="imgStyle">
   
  </div>
</template>

<script lang='ts' setup>
import { reactive ,computed} from 'vue'
import img1 from '../../assets/images/01.png'
const img = new Image()
img.src = img1
//输出图片宽高
const imgInfo = reactive({
  width: 0,
  height: 0
})
img.onload = function () {
  imgInfo.width = img.width
  imgInfo.height = img.height
  console.log('imgInfo: ', imgInfo);
}
//转换样式
const imgStyle = computed(() => {
  const { width, height } = imgInfo
  return {
    width: `${width}px`,
    height: `${height}px`,
    backgroundImage: `url(${img1})`,
    backgroundPosition: 'center',
  }
})

</script>
<style scoped lang='scss'>
.img-wrap{
  border:1px solid #ccc;
}
</style>