<template>
  <div>
    <p>会员专区模块</p>
    <div>我这里有好多好多会员干货</div>
    <div>隔壁的非会员模块肯定是没有的..</div>
    <p>{{title}}</p>
  </div>
</template>
<script>
export default {
  name: "memberInfo",
  props: ["title"],
  mounted() {
    console.log("会员mounted");
  },
  beforeDestroy() {
    console.log("会员beforeDestroy");
  },
  destroyed() {
    console.log("会员destroyed");
  },
  beforeCreate() {
    console.log("会员beforeCreate");
  },
  created() {
    console.log("会员created");
  }
};
</script>

