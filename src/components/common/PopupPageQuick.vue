<!--
    TODO 不可用

    props:
    height, popup内容的高度
    hidden, 将hidden从false 置为true，组件将显示隐藏动画，动画执行完毕后，触发unRender事件

    events:
    unRender, 隐藏该popup事件

    注意: 该组件(或该组件的父，祖组件)必须绑定v-if
    注意: 该组件hidden事件传出后，必须使用v-if销毁该组件

    @author herbluo
    change logs:
    2017/5/26 herbluo created
-->
<template>
  <div class="top">
    <div ref="bg" class="bg-modal" @click="onBgClick"></div>
    <div ref="content" class="content" :style="{width: '750px', height: height + 'px'}">
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
  .top {
  }

  .bg-modal {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;

    width: 750px;

    opacity: 0;
    background-color: #000;
  }

  .content {
    position: fixed;
    bottom: 0;
    left: 0;

    background-color: #fff;
    opacity: 0;
  }
</style>

<script>
  import {platform} from '../../utils/global'

  const animation = weex.requireModule('animation')

  export default {
    name: 'popup-page-quick',
    props: {
      height: {
        type: Number,
        'default': 850
      },
      hidden: {
        type: Boolean,
        'default': false
      }
    },
    data () {
      return {
        platform
      }
    },
    watch: {
      hidden () {
        this.hidden === true
          ? this.onBgClick()
          : this.show()
      }
    },
    mounted () {
      this.prepare()
    },
    methods: {

      prepare () {
        const bg = this.$refs['bg']
        const content = this.$refs['content']

        // 将内容下移 X 像素， X为内容的高度
        animation.transition(
          content,
          {
            styles: {
              transform: `translateY(${this.height})`
            }
          },
          // 将内容 设置为 不透明
          () => {
            animation.transition(
              content,
              {
                styles: {opacity: 1}
              }
            )
          }
        )

      },

      /**
       * 动画：
       * 组件初始化完毕后，该组件为全透明组件
       * 将该组件移除屏幕后，设为不透明组件
       */
      show () {
        const bg = this.$refs['bg']
        const content = this.$refs['content']

        animation.transition(bg, {styles: {opacity: 0.6}, duration: 150})

        animation.transition(
          content,
          {
            styles: {transform: 'translateY(0)'},
            duration: (this.height / 3.4) | 0,
            timingFunction: 'ease-in-out'
          }
        )

      },

      hide (callback) {
        const bg = this.$refs['bg']
        const content = this.$refs['content']

        animation.transition(bg, {styles: {opacity: 0}, duration: 200})

        animation.transition(
          content,
          {
            styles: {transform: `translateY(${this.height}px)`},
            duration: (this.height / 2.5) | 0
          },
          callback
        )
      },

      onBgClick () {
        this.hide(() => {
          this.$emit('unRender')
        })
      }
    }
  }
</script>

<style></style>