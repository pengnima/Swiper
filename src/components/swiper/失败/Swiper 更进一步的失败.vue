<template>
  <div id="pnm_swiper">
    <div
      class="swiper"
      @touchstart="touchStart"
      @touchmove="touchMove"
      @touchend="touchEnd"
    >
      <slot></slot>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      currIndex: 1,
      slideCount: 0,
      swiperStyle: {}, //swiper样式
      slideWidth: null, //一张slide的宽度
      moveTimer: null,
      startPos: 0,
      movePos: 0,
      distance: 0,
      scrolling: false
    };
  },
  props: {
    moveRatio: {
      type: Number,
      default: 0.2
    },
    animDuration: {
      type: Number,
      default: 300
    }
  },
  mounted() {
    setTimeout(() => {
      this.initDom();
      this.moveAuto();
    }, 100);
  },
  methods: {
    //1.初始化DOM，并获取必要的数据
    initDom() {
      let swiperEl = document.querySelector(".swiper");
      let slides = swiperEl.querySelectorAll(".slide");
      this.swiperStyle = swiperEl.style;
      this.slideCount = slides.length;

      //如果slider数量大于1，那么往前后插入图片，方便滑动操作
      if (this.slideCount > 1) {
        let firstSlide = slides[0].cloneNode(true);
        let lastSlide = slides[this.slideCount - 1].cloneNode(true);
        swiperEl.appendChild(firstSlide);
        swiperEl.insertBefore(lastSlide, slides[0]);
      }
      this.slideWidth = swiperEl.offsetWidth;
      //设置开头位置
      this.setTransform(-this.slideWidth);
      //监听transition结束
      swiperEl.addEventListener("transitionend", this.transAnimaEnd);
    },

    //计算滑动位移
    setTransform(pos) {
      this.swiperStyle.transform = `translateX(${pos}px)`;
    },

    //自动滑动
    moveAuto() {
      this.moveTimer = setInterval(() => {
        this.currIndex++;
        this.scrolling = true;
        console.log(this.scrolling);
        this.swiperStyle.transition = "transform " + this.animDuration + "ms";
        this.setTransform(-this.currIndex * this.slideWidth);
      }, 1000);
    },
    //6.触屏事件
    touchStart(e) {
      console.log(this.scrolling);
      if (this.scrolling) return;
      clearInterval(this.moveTimer);
      this.startPos = e.touches[0].pageX;
    },

    touchMove(e) {
      if (this.scrolling) return;
      this.movePos = e.touches[0].pageX;
      this.distance = this.movePos - this.startPos;
      let moveDistance = -(this.currIndex * this.slideWidth) + this.distance;
      this.setTransform(moveDistance);
    },

    touchEnd(e) {
      let _Dis = Math.abs(this.distance); //绝对值求出移动的距离

      if (this.distance == 0) {
        return;
      } else if (this.distance > 0 && _Dis > this.slideWidth * this.moveRatio) {
        this.currIndex--;
      } else if (this.distance < 0 && _Dis > this.slideWidth * this.moveRatio) {
        this.currIndex++;
      }
      this.swiperStyle.transition = "transform " + this.animDuration + "ms";
      this.setTransform(-(this.currIndex * this.slideWidth));

      /* setTimeout(() => {
        if (this.currIndex >= this.slideCount + 1) {
          this.swiperStyle.transition = "0s";
          this.currIndex = 1;
          this.setTransform(-this.slideWidth);
        } else if (this.currIndex <= 0) {
          this.swiperStyle.transition = "0s";
          this.currIndex = this.slideCount;
          this.setTransform(-this.currIndex * this.slideWidth);
        }
      }, this.animDuration); */

      if (this.scrolling == false) this.moveAuto(); //1秒后才执行
    },

    //5.动画结束判断，利用监听 transitionend的方式，来判断是否要转到第一张图
    transAnimaEnd() {
      this.swiperStyle.transition = "0s";
      //当要显示最后一张图片时
      if (this.currIndex >= this.slideCount + 1) {
        this.currIndex = 1;
        this.setTransform(-this.slideWidth);
      } else if (this.currIndex <= 0) {
        this.currIndex = this.slideCount;
        this.setTransform(-this.currIndex * this.slideWidth);
      }
      this.scrolling = false;
      console.log("调用一次");
    }
  }
};
</script>
<style>
#pnm_swiper {
  overflow: hidden;
  position: relative;
}

.swiper {
  display: flex;
}
</style>
