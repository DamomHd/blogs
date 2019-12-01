<template>
  <div>
    <p>非会员专区模块</p>
    <p>嘤嘤嘤，我什么都没有...</p>

    <p>{{title}}</p>
  </div>
</template>
<script>
export default {
  name: "userInfo",
  props: ["title"],
  mounted() {
    console.log("非会员mounted");
  },
  beforeDestroy() {
    console.log("非会员beforeDestroy");
  },
  destroyed() {
    console.log("非会员destroyed");
  },
  beforeCreate() {
    console.log("非会员beforeCreate");
  },
  created() {
    console.log("非会员created");
  }
};
</script>

