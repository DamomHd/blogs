<template>
  <div class="container">
    <button @click="isMember = !isMember">{{isMember?'我不想要会员了，哼':'我要成为会员'}}</button>
    <UserInfo v-if="!isMember"/>
    <MemberInfo v-else/>
  </div>
</template>

<script>
import UserInfo from "../components/UserInfo";
import MemberInfo from "../components/MemberInfo";

export default {
  name: "user",
  data() {
    return {
      isMember: false
    };
  },
  components: {
    UserInfo,
    MemberInfo
  }
};
</script>
<style scoped>
.container {
  margin-top: 100px;
}
button {
  margin-bottom: 100px;
}
</style>
