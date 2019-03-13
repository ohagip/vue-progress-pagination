# vue-progress-pagination
プログレスバーでカウントダウンするページネーション  
スライダーに利用

## Install
```
npm i --save gsap
```

### components file
`./src/components/ProgressPagination.vue`  
`./src/components/ProgressPaginationBullet.vue`

## Usage
```
<ProgressPagination :items="paginationItems" :change="onChangePagination" />
```

[DEMO](https://ohagip.github.io/vue-progress-pagination/)

## Documentation

### paginationItems
```
paginationItems: [
  {
    duration: 5, // スライドの時間
    enter: next => { next(); }, // スライド開始時のcallback, 処理完了後にnextを実行
    leave: next => { next(); }  // スライド完了時のcallback, 処理完了後にnextを実行
  },
  { duration: 4 },
  { duration: 5 }
]
```

### onChangePagination
スライド切り替えの処理を行う
```
/**
 * @param nextIndex - 次のスライドのインデックス
 * @param prevIndex - 前のスライドのインデックス
 * @param next - 処理完了後に実行
 */
onChangePagination(nextIndex, prevIndex, next) {
  next();
}
```

---
# Vue CLI npm script

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Run your tests
```
npm run test
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
