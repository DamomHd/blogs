<template>
  <div>
    <p>
      {{info.title}}
      <span>{{info.startTime}}</span>
    </p>
    <div>
      <img src="../assets/Bargain.png" alt>
    </div>
  </div>
</template>
<script>
export default {
  name: "bargain",
  props: ["info"],
  computed: {}
};
</script>

<style scoped>
p {
  text-align: left;
}
</style>