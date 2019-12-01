<template>
  <div>
    <p>
      {{info.title}}
      <span>{{info.startTime}}</span>
    </p>
    <div>
      <img src="../assets/Seckill.png" alt>
    </div>
  </div>
</template>
<script>
export default {
  name: "seckill",
  props: ["info"]
};
</script>

<style scoped>
p {
  text-align: left;
}
</style>
