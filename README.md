# vue-progress-pagination
プログレスバーでカウントダウンするページネーション  
スライダーに利用  

[DEMO](https://ohagip.github.io/vue-progress-pagination/)

## Dependencies
```
npm i --save gsap
```

## Install
`./src/components/ProgressPagination.vue`  
`./src/components/ProgressPaginationBullet.vue`  
をコピペ、スタイルなど適宜調整

## Usage
```
<ProgressPagination :items="paginationItems" :change="onChangePagination" />
```

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

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```