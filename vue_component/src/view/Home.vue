<template>
  <div>
    <p>假装这是一个商城首页</p>
    <button @click="shuffle">随机切换模块顺序</button>
    <component :is="item.imstance" v-for="(item ,i) in componentImstances" :info="item" :key="i"/>
  </div>
</template>

<script>
export default {
  name: "home",
  data() {
    return {
      moduleList: [
        {
          type: "Bargain",
          title: "砍价",
          startTime: "2019-12-01",
          endTime: "2019-01-01"
        },
        {
          type: "Seckill",

          title: "秒杀",
          startTime: "2019-12-05",
          endTime: "2019-01-01"
        },
        {
          type: "Limit",

          title: "限购",
          startTime: "2019-12-07",
          endTime: "2019-01-01"
        },
        {
          type: "Group1",
          title: "团购",
          startTime: "2019-12-11",
          endTime: "2019-01-01"
        }
      ]
    };
  },
  components: {},
  methods: {
    shuffle() {
      let { moduleList } = this;
      let resultArr = [];
      while (moduleList.length > 0) {
        var index = Math.floor(Math.random() * moduleList.length);
        resultArr.push(moduleList[index]);
        moduleList.splice(index, 1);
      }
      this.moduleList = resultArr;
    }
  },
  computed: {
    componentImstances() {
      let { moduleList } = this;
      return moduleList.map(item => {
        // item.imstance = () => import(`../components/${item.type}`);
        item.imstance = () => {
          return new Promise((resove, reject) => {
            let imstance = import(`../components/${item.type}`);
            imstance.then(res => {
              resove(res);
            });
            imstance.catch(e => {
              resove(import("../components/Error"));
            });
          });
        };
        return item;
      });
    }
  }
};
</script>
<style >
img {
  display: inline-block;
  width: 30%;
  height: auto;
}
</style>
