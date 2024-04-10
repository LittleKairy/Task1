<template>
  <div class="container">
    <div class="icon-left" @click="throttle(handleLeftClick, 1000)">
      <i class="iconfont icon-qiehuanqizuo"></i>
    </div>

    <div
      class="avatar-list"
      ref="list"
      draggable="true"
      @dragstart.prevent
      :style="{
        transform: `translateX(${left}px)`,
        transition: `${transition}s`,
      }"
      @mousedown="handleMousedown"
      @mouseup="handleMouseup"
      @mousemove="handleMousemove"
      @mouseleave="handleMouseleave"
      @touchstart="handleTouchstart"
      @touchend="handleTouchend"
      @touchmove="handleTouchmove"
      @touchleave="handleTouchleave"
    >
      <Avatar v-for="item in avatarList" :key="item.id" />
    </div>

    <div class="icon-right" @click="throttle(handleRightClick, 1000)">
      <i class="iconfont icon-qiehuanqiyou"></i>
    </div>
  </div>
</template>

<script>
import Avatar from "@/components/Avatar";
export default {
  components: {
    Avatar,
  },
  data() {
    return {
      // 这里模拟服务器传过来的人物数据
      avatarList: [
        { id: 1, name: "1" },
        { id: 2, name: "2" },
        { id: 3, name: "3" },
        { id: 4, name: "4" },
        { id: 5, name: "5" },
        { id: 6, name: "6" },
        { id: 7, name: "7" },
        { id: 8, name: "8" },
        { id: 9, name: "9" },
        { id: 10, name: "10" },
        { id: 11, name: "11" },
        { id: 12, name: "12" },
      ],
      total: 12, // 一共12条数据
      clientWidth: 0, // 视口宽度
      listWidth: 0, // 列表宽度
      avatarWidth: 140, // 每个头像的宽度
      left: 0, // 列表的margin-left
      isDrag: false, // 是否在拖拽
      preMouseX: 0, // 开始拖拽时的鼠标位置
      dragLength: 0, // 拖拽的长度
      transition: 1,
      timerId: null, // 节流用
    };
  },
  mounted() {
    this.clientWidth = window.innerWidth;
    this.listWidth = this.$refs.list.clientWidth;
    console.log(this.listWidth);
    console.log(this.curNums);
    window.addEventListener("resize", () => {
      this.clientWidth = window.innerWidth;
      console.log(this.clientWidth);
      console.log(this.curNums);
    });
  },
  destroyed() {
    window.removeEventListener("resize", () => {
      this.clientWidth = window.innerWidth;
      console.log(this.clientWidth);
      console.log(this.curNums);
    });
  },
  computed: {
    // 当前页面上显示了几个头像
    curNums() {
      const num = Math.floor((this.clientWidth - 120) / this.avatarWidth);
      console.log(num);
      return num;
    },
    // 点击按钮，一次可以移动的距离
    moveLength() {
      // 可能最后面的头像没有显示完全，这里少移动一个
      if (this.curNums > 1) return (this.curNums - 1) * this.avatarWidth;
      else return this.curNums * this.avatarWidth;
    },
  },
  methods: {
    handleRightClick() {
      console.log("right");

      // 判断下一次向右是否可以到达列表末尾
      // console.log(
      //   this.listWidth,
      //   this.left,
      //   this.moveLength,
      //   this.listWidth + this.left - this.moveLength,
      //   this.clientWidth - 120 * 2
      // );
      if (
        this.listWidth + this.left - this.moveLength <=
        this.clientWidth - 120 * 2
      ) {
        console.log("到达最右边");
        this.left =
          -this.moveLength +
          (this.clientWidth - 120 * 2 + this.moveLength - this.listWidth);
      } else {
        this.left -= this.moveLength;
      }
    },
    handleLeftClick() {
      console.log("left");

      // 判断下一次向左是否到达开头
      if (this.left + this.moveLength >= 0) {
        console.log("到达最左边");
        this.left = 0;
      } else {
        this.left += this.moveLength;
      }
    },
    throttle(fn, wait) {
      if (!this.timerId) {
        fn();
        this.timerId = setTimeout(() => {
          this.timerId = null;
        }, wait);
      }
    },
    changeLeft() {
      if (this.left + this.dragLength >= 0) {
        console.log("不能继续向左拖动了");
      } else {
        // 判断是否到达最有端
        if (
          this.left + this.dragLength + this.listWidth <=
          this.clientWidth - 120 * 2
        ) {
          console.log("不能继续向右拖动了");
        } else {
          // 可以拖动
          this.left += this.dragLength;
        }
      }
    },
    handleMousedown(e) {
      console.log("鼠标按下", e);
      this.preMouseX = e.screenX;
      this.isDrag = true;
      this.transition = 0;
    },
    handleMouseup(e) {
      console.log("鼠标抬起", e);
      this.dragLength = e.screenX - this.preMouseX;
      this.isDrag = false;
      this.transition = 1;
    },
    handleMousemove(e) {
      // console.log("鼠标移动", e);
      if (!this.isDrag) return;

      this.dragLength = e.screenX - this.preMouseX;
      this.preMouseX = e.screenX;
      // console.log(this.dragLength);

      this.changeLeft();
    },
    handleMouseleave(e) {
      this.isDrag = false;
      this.transition = 1;
    },
    handleTouchstart(e) {
      console.log("手指触摸开始", e);
      this.isDrag = true;
      this.transition = 0;

      this.preMouseX = e.changedTouches[0].screenX;
    },
    handleTouchend(e) {
      console.log("手指触摸结束", e);
      this.isDrag = false;
      this.transition = 1;
    },
    handleTouchmove(e) {
      // console.log("手指移动", e);

      this.dragLength = e.changedTouches[0].screenX - this.preMouseX;
      this.preMouseX = e.changedTouches[0].screenX;
      // console.log(this.dragLength);

      this.changeLeft();
    },
    handleTouchleave(e) {
      this.isDrag = false;
      this.transition = 1;
    },
  },
};
</script>

<style scoped>
.container {
  width: 100vw;
  height: 200px;
  position: relative;
  background: #ffc861;
  /* padding: 0 calc(2% + 32px); */
  box-sizing: border-box;
  display: flex;
  align-items: center;
  overflow: hidden;
}

.avatar-list {
  height: 100%;
  box-sizing: border-box;
  padding: 25px 0;
  display: flex;
  align-items: center;
  margin-left: 120px;
  /* transition: 0.8s linear; */
}

.icon-left,
.icon-right {
  height: 100%;
  width: 150px;
  position: absolute;
  z-index: 1;
}

.icon-left {
  left: 0;
  top: 0;
  background: rgb(255, 200, 97);
  background: linear-gradient(
    90deg,
    rgba(255, 200, 97, 1) 33%,
    rgba(255, 200, 97, 0.7053321817008054) 63%,
    rgba(255, 200, 97, 0) 100%
  );
}

.icon-right {
  top: 0;
  right: 0;
  background: rgb(255, 200, 97);
  background: linear-gradient(
    90deg,
    rgba(255, 200, 97, 0) 0%,
    rgba(255, 200, 97, 0.7053321817008054) 37%,
    rgba(255, 200, 97, 1) 67%
  );
}

.iconfont {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 2em;
  color: #fdf3a0;
  transition: 0.5s;
}

.iconfont:hover {
  transform: translate(-50%, -50%) scale(1.2);
  color: #fff;
  cursor: pointer;
}
</style>
