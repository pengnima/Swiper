### 【注】弹性盒子

- 弹性盒子 display: flex; 只针对子元素！

```
  .father {
    display: flex;
  }

  .sun {
    width: 100%;
    flex-shrink: 0;
  }
  //【注】由于弹性盒子是针对子元素的，所以 .sun需要并排，而不能嵌套在里面
```

---

### 1.关于 transform 在父元素上进行 translate 平移操作

- 该操作会移动父元素，子元素会因为父元素的移动而移动

---

### 2.在过渡动画结束后执行代码的方式：

1. 可以利用 transitionend 事件。
2. 可以利用 setTimeout(()=>{},过渡时间)
   > 建议用第二种，第一种如果遇到了平繁过渡的情况，会无法准确匹配到 transitionend 事件

---

### 3.绑定类，样式，也可以利用函数，但返回的时候，需要 return { className == bool}
