<template>
  <div class="container" @mouseover="clearInv" @mouseout="runInv" :style="myheight">
    <ul class="sliderImg">
      <!-- <li v-for="(item, index) in sliders" :key="index">
        <img :src="item.img">
      </li>  -->
      <transition name="slideMove">
        <img  v-if="isShow" :src="sliders[currentIndex].img" :style="myheight">
      </transition>
      <transition name="slideMove-old">
        <img  v-if="!isShow" :src="sliders[currentIndex].img">
      </transition>
    </ul>
    <div class="left btn" v-if="btnShow" @click="move(prevIndex)"> &lt; </div>
    <div class="right btn" v-if="btnShow" @click="move(nextIndex)"> &gt; </div>
    <ul class="dots">
      <li v-for="(item, index) in sliders" :key="index" @click="move(index)" :class="{on: index === currentIndex}"></li>
    </ul>
  </div>
</template>

<script>
//vue-cli里的webpack会自动在渲染img的src属性时将图片路径变成字符串，非真实路径
//解决方法：
//1. 将静态图片放入static文件夹下
//2. 利用commonjs规范require来解决图片路径问题
// img: require('../assets/slide01.jpg')
export default {
  data() {
    return {
      // sliders:[
      //  {
      //    img: "/static/slide01.jpg"
      //  },{
      //    img: "/static/slide03.jpg"
      //  },{
      //    img: "/static/slide01.jpg"
      //  },{
      //    img: "/static/slide03.jpg"
      //  }
      // ],
      sliders:[
        {
          img: require("../assets/slide01.jpg")
        },{
          img: require("../assets/slide03.jpg")
        },{
          img: require("../assets/slide01.jpg")
        },{
          img: require("../assets/slide03.jpg")
        }
      ],
      currentIndex:0,
      isShow:true,
      btnShow:true,
      interval:4000,
      mywidth: window.innerWidth
    }
  },
  computed: {
    prevIndex() {
      if(this.currentIndex === 0) {
        return this.sliders.length - 1
      } else {
        return this.currentIndex - 1
      }
    },
    nextIndex() {
      console.log(this.mywidth)
      if(this.currentIndex === this.sliders.length - 1) {
        return 0
      } else {
        return this.currentIndex + 1
      }
    },
    myheight() {
      return {
        height: (this.mywidth * (55 / 1707)) + "vh"
      }
    }
  },
  methods: {
    move(index) {
      this.isShow = false
      setTimeout(() => {
        this.isShow = true
        this.currentIndex = index
      }, 10)
    },
    runInv() {
      this.btnShow = false
      this.invTimer = setInterval(() => {
        this.move(this.nextIndex)
      }, this.interval)
    },
    clearInv() {
      this.btnShow = true
      clearInterval(this.invTimer)
    }
  },
  watch: {
    // 另一种监听window.innerWidth方法
    // mywidth(val) {
    //  this.mywidth = val
    // }
  },
  mounted() {
    this.runInv()
    window.onresize = () => {
      return (() => {
        this.mywidth = window.innerWidth
      })()
    }
  }
}

</script>

<style scoped>
  /* 动画 */
  .container .sliderImg {
    /* perspective: 200;  */
  }
  .slideMove-enter-active {
    transition: all 1.5s ease;
    transform: translateX(0);
  }
  .slideMove-enter {
    transform: translateX(100vw);
    /* transform-style:preserve-3d;  */
    transform: rotateY(90deg);  
  }
  .slideMove-leave {
    transform: translateX(0);
  }
  .slideMove-old-leave-active {
    transition: all 1.5s ease;
    transform: translateX(-100vw);
  }

  .container {
    position: relative;
    width: 100%;
    height: 55vh;
    overflow: hidden;
  } 
  .container .sliderImg img {
    /* clip-path: inset(0px 0px 200px); */
    width: 100vw;
    height: 80vh;
  }
  .btn {
    position: absolute;
    padding: 5px;
    top: 50%; 
    border-radius: 50%;
    background-color: #fff;
    cursor: pointer;
  }
  .btn.left {
    left: 4%;
  }
  .btn.right {
    right: 4%;
  }
  .dots {
    position: absolute;
    bottom: 20px;
    right: 3%;    
    text-align: right;
    padding: 5px 3px;
    border-radius: 20px;
    background-color: #000;
    opacity: .4;
  }
  .dots li {
    display: inline-block;
    width: 10px;
    height: 10px;
    border-radius: 50%;
    margin-right: 10px;
    cursor: pointer;
    background-color: #fff;
  }
  .dots li:nth-child(1) {
    margin-left: 10px;
  }
  .dots li.on {
    background-color: skyblue;
  }
</style>