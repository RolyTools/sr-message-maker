<template>
  <Transition
    name="window"
    appear
  >
    <div
      class="window"
      v-if="show"
    >
      <div
        class="box"
        :style="{
          width,
          height
        }"
        @click.stop
      >
        <div class="outside">
          <slot name="outside"></slot>
        </div>
        <div class="wrapper">
          <div
            class="left"
            v-if="slot.left"
          >
            <slot name="left"></slot>
          </div>
          <div class="right">
            <div
              class="title"
              :style="{ justifyContent: confirm ? 'center' : undefined }"
            >
              <span>
                {{ title }}
              </span>

              <Close
                v-if="!!onClose && !confirm"
                class="close"
                @click="close"
              />
            </div>
            <div class="item">
              <slot></slot>
            </div>
            <slot name="bottom"></slot>
          </div>
        </div>
        <div
          class="footer"
          v-if="slot.footer"
        >
          <div class="bg"></div>
          <div class="btn-list">
            <slot name="footer"></slot>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script lang="ts" setup>
import Close from './Close.vue'

defineProps<{
  show: boolean
  confirm?: boolean
  title: string
  width?: string
  height?: string
  onClose?: () => void
}>()

const slot = defineSlots()

const emit = defineEmits<{
  (event: 'close'): void
}>()

const close = () => {
  emit('close')
}
</script>

<style lang="stylus" scoped>
@import './Window.styl'

.window
  z-index 99
  display flex
  justify-content center
  align-items center
  position absolute
  top 0
  left 0
  width 100%
  height 100%
  backdrop-filter blur(10px)
  -webkit-backdrop-filter blur(10px)
  background rgba(0, 0, 0, 0.2)

  .box
    position relative
    display flex
    flex-direction column
    width fit-content
    width -moz-fit-content
    max-width 90%
    height fit-content
    height -moz-fit-content
    max-height 90%
    background var(--box-background-color)
    message()

    .outside
      position absolute
      left -150px
      top 0

    .wrapper
      overflow hidden
      display flex
      width 100%
      border-radius 0 50px 0 0

      .left
        padding 20px

      .right
        flex 1

        .title
          display flex
          align-items flex-end
          justify-content space-between
          height 85px
          font-size 60px
          font-weight bold
          border-bottom 5px solid rgba(150, 150, 150, 0.5)
          margin 0 80px
          padding 40px 0 30px 0
          user-select none

          .close
            margin-left 20px

        .item
          overflow auto
          display flex
          flex-direction column
          margin 40px 80px
          max-height calc(100% - 80px * 2 - 40px - 30px)

    .footer
      position relative
      display flex
      align-content center
      justify-content center
      padding 30px 80px
      background var(--box-background-color)

      .bg
        position absolute
        top 0
        right 0
        bottom 0
        left 0
        background #262626
        background-image url('@/assets/images/菜单背景.webp')
        background-position center
        background-size auto 100%
        background-repeat no-repeat
        display flex
        align-content center
        justify-content center

      .btn-list
        display flex
        align-content center
        justify-content center

.window-enter-active
  transition all 0.3s

.window-leave-active
  transition all 0.25s

.window-enter-from, .window-leave-to
  opacity 0

.window-enter-active .box, .window-leave-active .box
  transition all 0.2s ease-out

.window-enter-from .box, .window-leave-to .box
  transform translateY(10%)

.window-enter-active .bg, .window-leave-active .bg
  transition all 0.25s
  transition-delay 0.05s
  transform-origin bottom

.window-enter-from .bg
  opacity 0
  transform scaleY(0)

.window-leave-to .bg
  opacity 0
  transform scaleY(0) translateY(30%)

.window-enter-active .btn-list
  transition all 0.35s
  transition-delay 0.15s

.window-enter-from .btn-list
  opacity 0

.window-leave-active .btn-list
  transition all 0.2s

.window-leave-to .btn-list
  opacity 0
</style>
