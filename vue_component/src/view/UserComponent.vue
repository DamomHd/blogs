<template>
  <div class="container">
    <button @click="isMember = !isMember">{{isMember?'我不想要会员了，哼':'我要成为会员'}}</button>
    <component :is="userComponentName" title="component就是好用哟"/>
  </div>
</template>

<script>
import UserInfo from "../components/UserInfo";
import MemberInfo from "../components/MemberInfo";

export default {
  name: "userComponent",
  data() {
    return {
      isMember: false
    };
  },
  components: {
    UserInfo,
    MemberInfo
  },
  computed: {
    userComponentName() {
      let { isMember } = this;
      return isMember ? "member-info" : "user-info";
    }
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
