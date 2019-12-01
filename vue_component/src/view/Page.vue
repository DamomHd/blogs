<template>
  <div>
    <Child msg="『 前端公虾米 』教你Hello World！"/>
  </div>
</template>

<script>
import Child from "../components/Child";

export default {
  name: "page",
  components: {
    Child
  }
};
</script>