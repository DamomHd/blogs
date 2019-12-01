<template>
  <div>
    <p>
      {{info.title}}
      <span>{{info.startTime}}</span>
    </p>
    <div>
      <img src="../assets/Group.png" alt>
    </div>
  </div>
</template>
<script>
export default {
  name: "group",
  props: ["info"]
};
</script>

<style scoped>
p {
  text-align: left;
}
</style>