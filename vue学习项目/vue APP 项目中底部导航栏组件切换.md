## 组件切换问题记录

```css

.app-container{

​    padding-top: 40px;

​    overflow-x: hidden;//防止底部和顶部滚动

  }

  .v-enter{

​    opacity: 0;

​    transform: translateX(100%); //让组件从右边进入

  }

   .v-leave-to{

​    opacity: 0;

​    transform: translateX(-100%); //让组件从左边离开

​    position: absolute;//防止组件进入后跳动一下

   }

  .v-enter-active,

  .v-leave-to{

​    transition: all 500ms  ease

  }
```

