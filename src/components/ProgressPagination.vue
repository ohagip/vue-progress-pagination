<template>
  <div class="pagination">
    <ProgressPaginationBullet
      class="pagination_bullet"
      v-for="(item, index) in items"
      :key="index + 1"
      :item="item"
      @click="onClick(index)"
      @finished="onFinished(index)"
    />
  </div>
</template>

<script>
import ProgressPaginationBullet from './ProgressPaginationBullet.vue';

export default {
  props: {
    items: Array,
    change: Function
  },
  components: {
    ProgressPaginationBullet
  },

  data() {
    return {
      currentIndex: null,
      length: this.items.length,
      isDisable: false
    }
  },
  methods: {
    onClick(index) {
      this.goto(index)
    },

    onFinished() {
      this.next();
    },

    next() {
      if (this.currentIndex >= this.length - 1) {
        this.goto(0);
      } else {
        this.goto(this.currentIndex + 1);
      }
    },

    prev() {
      if (this.currentIndex <= 0) {
        this.goto(this.length - 1);
      } else {
        this.goto(this.currentIndex - 1);
      }
    },

    async goto(nextIndex) {
      if (nextIndex < 0 ||
        nextIndex === this.currentIndex ||
        nextIndex > this.length - 1 ||
        this.isDisable === true) {
        return;
      }

      this.isDisable = true;

      const p = [];

      // tween中のコンポーネントがある場合
      if (this.currentIndex !== null &&
        this.$children[this.currentIndex].isTween === true) {
        p.push(new Promise(resolve => {
          if (this.currentIndex < nextIndex) {
            this.$children[this.currentIndex].state = 'finished';
          } else {
            this.$children[this.currentIndex].state = 'wait';
          }
          setTimeout(resolve, 300);
        }));
      }

      // クリックで順でないindexへ移った場合、オートにより0番目へ移った場合
      if (this.currentIndex !== null && nextIndex === 0 ||
        this.currentIndex + 1 !== nextIndex) {
        p.push(new Promise(resolve => {
          for (let i = 0; i < this.length; i += 1) {
            if (i < nextIndex) {
              this.$children[i].state = 'finished';
            } else {
              this.$children[i].state = 'wait';
            }
          }
          setTimeout(resolve, 200);
        }));
      }
      // 順なindexへ移った場合
      else {
        this.$children[this.currentIndex].state = 'finished';
      }

      if (p.length > 0) {
        await Promise.all(p);
      }

      if (this.currentIndex !== null &&
        typeof this.items[this.currentIndex].leave === 'function') {
        await new Promise(resolve => {
          this.items[this.currentIndex].leave(resolve);
        });
      }

      if (this.currentIndex !== null &&
        typeof this.change === 'function') {
        await new Promise(resolve => {
          this.change(nextIndex, this.currentIndex, resolve);
        });
      }

      if (typeof this.items[nextIndex].enter === 'function') {
        await new Promise(resolve => {
          this.items[nextIndex].enter(resolve);
        });
      }

      this.currentIndex = nextIndex;
      this.$children[nextIndex].state = 'progress';
      this.isDisable = false;
    },

    start() {
      this.goto(0)
    }
  },

  mounted() {
    this.start();
  }
};
</script>

<style lang="scss" scoped>
.pagination {
  display: inline-block;
  font-size: 0;
  text-align: center;
}

.pagination_bullet {
  margin: 0 4px;
}
</style>
