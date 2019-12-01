<template>
  <div>
    <p>{{info.title}}活动模块加载异常</p>
    <img src="../assets/error.jpg" alt>
  </div>
</template>
<script>
export default {
  name: "error",
  props: ["info"]
};
</script>
<style scoped>
img{
  width: 60%;
}
</style>


