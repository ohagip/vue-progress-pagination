<template>
  <div
    class="paginationBullet"
    :class="{ 'is-progress': isProgress }"
    @click="$emit('click')"
  >
    <div class="paginationBullet_bar" ref="bar"></div>
  </div>
</template>

<script>
import { TweenMax, Power0, Power2 } from 'gsap';

export default {
  props: {
    item: Object
  },

  data() {
    return {
      state: 'wait', // wait, progress, finished
      tween: null // TweenMax Object
    }
  },

  computed: {
    isProgress() {
      return this.state === 'progress';
    },

    isTween() {
      return this.tween !== null;
    }
  },

  methods: {
    wait() {
      if (this.isTween === true) {
        this.killTween();
        TweenMax.to(this.$refs.bar, 0.3, {
          x: '-100%',
          opacity: 0,
          ease: Power2.easeOut,
          onComplete: this.killTween
        });
      } else {
        TweenMax.to(this.$refs.bar, 0.2, {
          opacity: 0,
          ease: Power0.easeNone,
          onComplete: this.killTween
        });
      }
    },

    progress() {
      const target = this.$refs.bar;
      const duration = this.item.duration;
      TweenMax.set(this.$refs.bar, { opacity: 1, x: '-100%' });
      this.tween = TweenMax.to(target, duration, {
        x: '0%',
        ease: Power0.easeNone,
        onComplete: () => {
          this.killTween();
          this.$emit('finished');
        },
      });
    },

    finished(nextState, prevState) {
      if (this.isTween === true) {
        this.killTween();
        TweenMax.to(this.$refs.bar, 0.3, {
          x: '0%',
          ease: Power2.easeOut,
          onComplete: this.killTween
        });
      } else if (prevState !== 'progress') {
        TweenMax.set(this.$refs.bar, { opacity: 0, x: '0%' });
        TweenMax.to(this.$refs.bar, 0.2, {
          opacity: 1,
          ease: Power0.easeNone,
          onComplete: this.killTween
        });
      }
    },

    killTween() {
      if (this.tween !== null) {
        this.tween.kill();
        this.tween = null;
      }
    }
  },

  watch: {
    state(nextState, prevState) {
      this[nextState](nextState, prevState);
    }
  }
};
</script>

<style lang="scss" scoped>
.paginationBullet {
  position: relative;
  overflow: hidden;
  display: inline-block;
  cursor: pointer;
  opacity: 1;
  transition: opacity 0.2s linear;
  height: 40px;
  width: 40px;

  &.is-progress {
    opacity: 1 !important;
    cursor: default;
  }

  &:hover {
    opacity: 0.8;
  }

  &:after {
    content: "";
    position: absolute;
    z-index: 1;
    display: block;
    left: 0;
    height: 1px;
    background-color: #909090;
    top: 20px;
    width: 40px;
  }
}

.paginationBullet_bar {
  position: absolute;
  z-index: 10;
  left: 0;
  height: 1px;
  background-color: #fff;
  top: 20px;
  width: 40px;
  opacity: 0;
}
</style>
