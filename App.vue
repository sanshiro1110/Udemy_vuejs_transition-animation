<template>
  <div class="main">
    <button @click="myAnimation = 'slide'">Slide</button>
    <button @click="myAnimation = 'fade'">Fade</button>
    <p>{{ myAnimation }}</p>
    <br>
    <button @click="add">追加</button>

    <transition-group name="fade" style="width: 200px; margin: auto;" tag="ul">
      <li
        style="cursor: pointer"
        v-for="(number, index) in numbers"
        :key="number"
        @click="remove(index)"
      >{{ number }}</li>
    </transition-group>

    <button @click="show=!show">切り替え</button>
    <br><br>
    <transition
      :css="false"
      @before-enter="beforeEnter"
      @enter="enter"
      @leave="leave"
    >
      <div class="circle" v-if="show"></div>
    </transition>
    <br>
    <button @click="myComponent = 'ComponentA'">ComponentA</button>
    <button @click="myComponent = 'ComponentB'">ComponentB</button>
    <transition name="fade" mode="out-in">
      <component :is="myComponent"></component>
    </transition>
    <transition name="fade" mode="out-in">
      <p v-if="show" key="bye">さようなら</p>
      <p v-else key="hello">こんにちは</p>
    </transition>
    <transition
      name="fade"
      appear
      enter-active-class="animate__animated animate__bounce"
      leave-active-class="animate__animated animate__flash"
    >
      <p v-if="show">hello</p>
    </transition>

    <transition
      :name="myAnimation"
      appear
    >
      <p v-if="show">bye</p>
    </transition>
  </div>
</template>

<script>
import ComponentA from './components/ComponentA.vue';
import ComponentB from './components/ComponentB.vue';

export default {
  data() {
    return {
      show: true,
      myAnimation: 'slide',
      myComponent: 'ComponentA',
      numbers: [0, 1, 2],
      nextNumber: 3,
    }
  },
  components: {
    ComponentA,
    ComponentB,
  },
  methods: {
    beforeEnter(el) {
      el.style.transform = 'scale(0)';
    },
    enter(el, done) {
      let scale = 0;
      const interval = setInterval(() => {
        el.style.transform = `scale(${scale})`;
        scale += 0.1;

        if(scale > 1) {
          clearInterval(interval);
          done();
        }
      }, 20);
    },
    // afterEnter(el) {},
    // enterCancelled(el) {},
    // beforeLeave(el) {},
    leave(el, done) {
      let scale = 1;
      const interval = setInterval(() => {
        el.style.transform = `scale(${scale})`;
        scale -= 0.1;
        if(scale < 0) {
          clearInterval(interval);
          done();
        }
      }, 20);
    },
    // afterLeave(el) {},
    // leaveCancelled(el) {},
    randomIndex() {
      return Math.floor(Math.random() * this.numbers.length);
    },
    add() {
      this.numbers.splice(this.randomIndex(), 0, this.nextNumber);
      this.nextNumber ++;
    },
    remove(index) {
      this.numbers.splice(index, 1);
    }
  }
}
</script>

<style scoped>
.main {
  width: 70%;
  margin: auto;
  padding-top: 5rem;
  text-align: center;
}

.circle {
  width: 200px;
  height: 200px;
  margin: auto;
  background-color: deeppink;
  border-radius: 100px;
}

.fade-move {
  transition: transform 0.5s;
}

.fade-enter {
  opacity: 0;
}
.fade-enter-active {
  transition: opacity 0.5s;
}
.fade-enter-to {
  opacity: 1;
}
.fade-leave {
  opacity: 1;
}
.fade-leave-active {
  transition: opacity 0.5s;
  position: absolute;
  width: 200px;
}
.fade-leave-to {
  opacity: 0;
}

.slide-enter,
.slide-leave-to {
  opacity: 0;
}
.slide-enter-to,
.slide-leave {
  opacity: 1;
}
.slide-enter-active {
  animation: slide-in 0.5s;
  transition: opacity 0.5s;
}
.slide-leave-active {
  animation: slide-in 0.5s reverse;
  transition: opacity 0.5s;
}

@keyframes slide-in {
  from {
    transform: translateX(100px);
  }
  to {
    transform: translateX(0);
  }
}
</style>
