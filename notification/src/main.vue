<template>
  <transition name="el-notification-fade">
    <!--    <div :class="['el-notification', customClass, horizontalClass]" v-show="visible" :style="positionStyle" @mouseenter="clearTimer()" @mouseleave="startTimer()" @click="click" role="alert">-->
    <div :class="['el-notification',typeStyle,customClass,horizontalClass]" v-show="visible" :style="positionStyle" @mouseenter="clearTimer()" @mouseleave="startTimer()" @click="click" role="alert">
      <v-icon :class="['el-notification__icon',iconColor]">{{typeClass}}</v-icon>
      <div class="el-notification__group" :class="{ 'is-with-icon': typeClass || iconClass }">
        <h2 class="el-notification__title" v-if="title">{{title}}</h2>
        <h2 class="el-notification__title" v-else>{{message}}</h2>
        <div class="el-notification__content" v-show="title">
          <slot>
            <p v-if="!dangerouslyUseHTMLString">{{ message }}</p>
            <p v-else>{{message}}</p>
          </slot>
        </div>
        <div class="el-notification__closeBtn" v-if="showClose" @click.stop="close">
          <v-icon>mdi-close-thick</v-icon>
        </div>
      </div>
    </div>
  </transition>
</template>

<script type="text/babel">
let typeMap = {
  // success: 'success',
  // info: 'info',
  // warning: 'warning',
  // error: 'error',
  success: 'check-circle-outline',
  info: 'alert',
  warning: 'head-alert-outline',
  error: 'close-circle'
};

export default {
  data () {
    return {
      visible: false,
      title: '',
      message: '',
      duration: 3000,
      type: '',
      showClose: true,
      customClass: '',
      iconClass: '',
      onClose: null,
      onClick: null,
      closed: false,
      verticalOffset: 0,
      timer: null,
      dangerouslyUseHTMLString: false,
      position: 'top-right'
      //  自定义弹出样式
      //   didiBg:'',          // 自定义弹框背景颜色
      //   didiIc:'',          // 自定义弹框图标颜色
      //   didiFc:''           // 自定义字体颜色
    };
  },

  computed: {
    iconColor () {
      return this.type ? `iconcolor-${this.type}` : 'iconcolor-default';
    },
    typeClass () {
      // console.log(this.iconClass);
      // console.log(this.type);
      // console.log(typeMap[this.type]);
      // console.log(typeMap['success']);
      // if (this.type) {
      //   console.log("type有值")
      //   return "mdi-"+typeMap['success']
      // } else {
      //   if (this.iconClass){
      //     console.log("type没值")
      //     return  "mdi-"+this.iconClass
      //   }
      // }
      return this.type ? `mdi-${typeMap[this.type]}` : `mdi-${this.iconClass}`;
      // return this.type && typeMap[this.type] ? `mdi-${typeMap[this.type]}` : '';
    },
    typeStyle () {
      return this.type ? `icon-${this.type}` : '';
    },
    horizontalClass () {
      return this.position.indexOf('right') > -1 ? 'right' : 'left';
    },

    verticalProperty () {
      return /^top-/.test(this.position) ? 'top' : 'bottom';
    },

    positionStyle () {
      return {
        [this.verticalProperty]: `${this.verticalOffset}px`
      };
    }
  },

  watch: {
    closed (newVal) {
      if (newVal) {
        this.visible = false;
        this.$el.addEventListener('transitionend', this.destroyElement);
      }
    }
  },

  methods: {
    destroyElement () {
      this.$el.removeEventListener('transitionend', this.destroyElement);
      this.$destroy(true);
      this.$el.parentNode.removeChild(this.$el);
    },

    click () {
      if (typeof this.onClick === 'function') {
        this.onClick();
      }
    },

    close () {
      this.closed = true;
      if (typeof this.onClose === 'function') {
        this.onClose();
      }
    },

    clearTimer () {
      clearTimeout(this.timer);
    },

    startTimer () {
      if (this.duration > 0) {
        this.timer = setTimeout(() => {
          if (!this.closed) {
            this.close();
          }
        }, this.duration);
      }
    },
    keydown (e) {
      if (e.keyCode === 46 || e.keyCode === 8) {
        this.clearTimer(); // detele 取消倒计时
      } else if (e.keyCode === 27) { // esc关闭消息
        if (!this.closed) {
          this.close();
        }
      } else {
        this.startTimer(); // 恢复倒计时
      }
    }
  },
  mounted () {
    if (this.duration > 0) {
      this.timer = setTimeout(() => {
        if (!this.closed) {
          this.close();
        }
      }, this.duration);
    }
    document.addEventListener('keydown', this.keydown);
  },
  beforeDestroy () {
    document.removeEventListener('keydown', this.keydown);
  }
};
</script>
<style lang="scss" scoped>
@import "../utils/style/notification.css";
</style>
