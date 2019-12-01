<template>
  <div class="container">
    <button @click="isMember = !isMember">{{isMember?'我不想要会员了，哼':'我要成为会员'}}</button>
    <component :is="userComponentImstance" title="component就是好用哟"/>
  </div>
</template>

<script>
export default {
  name: "userImport",
  data() {
    return {
      isMember: false
    };
  },
  mounted() {
    this.userComponentImstance()
      .then(() => {
        console.log(2);
      })
      .catch(() => {
        console.log(1);
      });
  },
  computed: {
    userComponentImstance() {
      let { isMember } = this;
      let pathName = isMember ? "MemberInfo" : "UserInfo";
      //通过import动态导入组件 配合webpack实现组件分离
      return () => import(`../components/${pathName}`);
      // return () => import(`../components/1`);
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
