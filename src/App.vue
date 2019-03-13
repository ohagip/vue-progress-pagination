<template>
  <div id="app">
    <button class="btn btn-prev" @click="prev"></button>
    <ProgressPagination
      class="pagination"
      ref="pagination"
      :items="paginationItems"
      :change="onChangePagination"
    />
    <button class="btn btn-next" @click="next"></button>
  </div>
</template>

<script>
  import ProgressPagination from './components/ProgressPagination.vue';

  export default {
    name: 'app',

    components: {
      ProgressPagination
    },

    data() {
      return {
        paginationItems: [
          {
            duration: 5,
            enter: next => {
              console.log('slide 1 enter');
              next();
            },
            leave: next => {
              console.log('slide 1 leave');
              setTimeout(next, 500)
            }
          },
          {
            duration: 4,
            enter: next => {
              console.log('slide 2 enter');
              setTimeout(next, 500)
            },
            leave: next => {
              console.log('slide 2 leave');
              next();
            }
          },
          { duration: 3 },
          { duration: 4 },
          { duration: 5 }
        ]
      };
    },

    methods: {
      onChangePagination(nextIndex, prevIndex, next) {
        console.log('onChangePagination', nextIndex, prevIndex)
        setTimeout(() => {
          next();
        }, 500)
      },

      prev() {
        this.$refs.pagination.prev();
      },

      next() {
        this.$refs.pagination.next();
      }
    }
  };
</script>

<style lang="scss" scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding: 20px;
  background-color: #000;
}

.pagination {
  vertical-align: middle;
}

.btn {
  position: relative;
  display: inline-block;
  margin: 0 40px;
  width: 30px;
  height: 30px;
  background-color: #fff;
  border-radius: 30px;
  vertical-align: middle;
  outline: none;
  cursor: pointer;
  transition: opacity .2s linear;

  &:hover {
    opacity: 0.8;
  }
}

.btn-prev:before {
  content: '';
  position: absolute;
  left: 10px;
  top: 8px;
  width: 10px;
  height: 10px;
  border-top: 2px solid #000;
  border-left: 2px solid #000;
  transform: rotate(-45deg);
}

.btn-next:before {
  content: '';
  position: absolute;
  left: 6px;
  top: 8px;
  width: 10px;
  height: 10px;
  border-top: 2px solid #000;
  border-right: 2px solid #000;
  transform: rotate(45deg);
}
</style>
