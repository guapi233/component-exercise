<template>
  <transition name="notice" appear>
    <div class="c-notice" v-if="isShow" :style="handleOffsetY" :class="[`at-${ position }`]">
      <i v-if="type" :class="`icon-${ type }`"></i>

      <div class="c-notice__wrapper">
        <h2 class="c-notice__title">{{ title }}</h2>
        <div class="c-notice__content" v-if="dangerouslyUseHTMLString" v-html="message"></div>
        <div class="c-notice__content" v-else>{{ message }}</div>
        <i class="c-notice__closeBtn icon-close" @click="destroy"></i>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "CNotice",
  data() {
    return {
      title: "提示", // 标题
      message: "提示信息", // 内容
      duration: 5000, // 展示时间
      isShow: true, // 是否展示
      offsetY: 0, // Y轴偏移量
      type: "", // 弹窗类型
      dangerouslyUseHTMLString: false, // 使用HTML插值
      position: "top-right", // 弹出位置
    };
  },
  computed: {
    // Y轴偏移量计算
    handleOffsetY() {
      let direction = this.position.split("-")[0];

      return { [direction]: 16 + this.offsetY + "px" };
    },
  },
  methods: {
    // 关闭
    destroy() {
      this.isShow = false;
    },
    // 计算Y轴偏移量
    calculateOffsetY() {
      // 已经存在的Notice组件
      let exsistNotices = document.querySelectorAll(
        `.c-notice.at-${this.position}`
      );

      Array.from(exsistNotices).forEach((item) => {
        this.offsetY += item.clientHeight + 16;
      });
    },
  },
  created() {
    // 定时关闭
    if (this.duration) {
      setTimeout(() => {
        this.isShow = false;
      }, this.duration);
    }

    // 计算Y轴偏移量
    this.calculateOffsetY();
  },
};
</script>

<style lang="scss">
.c-notice {
  display: flex;
  width: 330px;
  padding: 14px 26px 14px 13px;
  border-radius: 8px;
  box-sizing: border-box;
  position: fixed;
  background-color: #fff;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  transition: opacity 0.3s, transform 0.3s, left 0.3s, right 0.3s, top 0.4s,
    bottom 0.3s;
  overflow: hidden;
  z-index: 2002;
  right: 16px;

  .c-notice__wrapper {
    margin-right: 8px;
    margin-left: 13px;

    .c-notice__title {
      font-weight: 700;
      font-size: 16px;
      color: #303133;
      margin: 0;
    }

    .c-notice__content {
      font-size: 14px;
      line-height: 21px;
      margin: 6px 0 0;
      color: #606266;
      text-align: justify;
    }

    .c-notice__closeBtn {
      position: absolute;
      top: 18px;
      right: 15px;
      cursor: pointer;
      color: #909399;
      font-size: 12px;
    }
  }
}

// 动画
.at-top-right.notice-enter,
.at-bottom-right.notice-enter {
  right: -330px;
}

.at-top-right.notice-enter-to,
.at-bottom-right.notice-enter-to {
  right: 16px;
}

.at-top-left.notice-enter,
.at-bottom-left.notice-enter {
  left: -330px;
}

.at-top-left.notice-enter-to,
.at-bottom-left.notice-enter-to {
  left: 16px;
}

.at-top-left {
  left: 16px;
  right: 0;
}

.at-bottom-left {
  left: 16px;
  right: atuo;
  top: auto;
  bottom: 16px;
}

.at-bottom-right {
  top: auto;
  bottom: 16px;
}

.notice-leave-to {
  opacity: 0;
}

// 状态色
.icon-success {
  color: rgb(82, 196, 26);
}

.icon-error {
  color: rgb(253, 107, 109);
}

.icon-tip {
  color: rgb(5, 207, 164);
}

.icon-warning {
  color: rgb(250, 173, 20);
}
</style>