<template>
  <div class="icon-hover-container">
    <div class="icon" v-for="(item, i) in iconList" :key="i">
      <div class="container">
        <i class="iconfont" :class="item.name"></i>
        <p class="title">{{ item.title }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      iconList: [
        { name: "icon-analysis", title: "统计" },
        { name: "icon-a-aroundprotection", title: "安全" },
        { name: "icon-audit", title: "报表" },
        { name: "icon-Availability", title: "检测" },
        { name: "icon-attacker", title: "战斗" },
      ],
    };
  },
};
</script>

<style scoped>
.icon-hover-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  /* justify-items: center;
  align-items: center; */
  /* background: red; */
  width: 300px;
  height: 200px;
  margin: 0 auto;
}

.icon {
  border: 1px solid #ccc;
  margin: 0 -1px -1px 0;
  box-sizing: border-box;
  position: relative;
}

.icon:hover {
  border-color: #7f9c20;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.05), 0 2px 3px 0 rgba(0, 0, 0, 0.03),
    0 0 0 1px #e0e0e0;
  z-index: 1;
}

.container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: #999;
}

.iconfont {
  font-size: 2em;
}
</style>
