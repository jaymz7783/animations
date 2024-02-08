<template>
  <button type="button" @click.prevent="flag = !flag">Toggle</button>
  <!-- fades main out and fades secondary in when flag is toggled and vice versa by default mode is
  set to in-out which means the new element will fade in and the old element will fade out -->
  <!-- <transition name="fade" mode="out-in">
    <h2 v-if="flag" key="main">Hello World!</h2>
    <h2 v-else key="secondary">Goodbye World!</h2>
  </transition> -->
  <!-- animation example -->
  <!-- <transition name="zoom" type="animation" appear>
    <h2 v-if="flag">Hello</h2>
  </transition> -->

  <!-- transition component via JS It is suggested to do CSS transitions over JS -->
  <!-- <transition
    @before-enter="beforeEnter"
    @enter="enter"
    @after-enter="afterEnter"
    @before-leave="beforeLeave"
    @leave="leave"
    @after-leave="afterLeave"
    :css="false"
  >
    <h2 v-if="flag">Hey</h2>
  </transition> -->
  <button @click="addItem">Add</button>

  <ul>
    <transition-group
      name="fade"
      enter-active-class="animate__animated animate__flipInX"
      leave-active-class="animate__animated animate__flipOutX"
    >
      <li v-for="(number, index) in numbers" :key="number" @click="removeItem(index)">
        {{ number }}
      </li>
    </transition-group>
  </ul>
  <!-- vue transitions on list -->
  <!-- <ul>
    <transition-group name="fade">
      <li v-for="(number, index) in numbers" :key="number" @click="removeItem(index)">
        {{ number }}
      </li>
    </transition-group>
  </ul> -->
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      flag: true,
      numbers: [1, 2, 3, 4, 5],
    }
  },
  methods: {
    addItem() {
      const num = Math.floor(Math.random() * 100 + 1)
      const index = Math.floor(Math.random() * this.numbers.length)
      this.numbers.splice(index, 0, num)
    },
    removeItem(index) {
      this.numbers.splice(index, 1)
    },
    beforeEnter(el) {
      el.style.opacity = 0
      el.style.transform = 'scale(0, 0)'
    },
    enter(el, done) {
      const animation = el.animate([{ transform: 'scale3d(0,0,0)' }, {}], { duration: 1000 })
      animation.onfinish = () => {
        done()
      }
    },
    afterEnter(el) {
      el.style.opacity = 1
      el.style.transform = 'scale(1, 1)'
    },
    beforeLeave(el) {
      el.style.opacity = 1
      el.style.transform = 'scale(1, 1)'
    },
    leave(el, done) {
      const animation = el.animate([{}, { transform: 'scale3d(0,0,0)' }], { duration: 1000 })
      animation.onfinish = () => {
        done()
      }
    },
    afterLeave(el) {
      el.style.opacity = 0
      el.style.transform = 'scale(0, 0)'
    },
  },
}
</script>

<style>
/* animating list using animate.css */
/* handles the animation for removing numbers from list */
.animate__flipOutX {
  position: absolute;
}

/* overwrites the duration set by animate.css */
.animate__animated {
  animation-duration: 0.5s;
}

/* animating list */
li {
  font-size: 22px;
  cursor: pointer;
}

/* handles the animation for adding number to list */
.fade-move {
  transition: all 0.5s linear;
}

/* handles the animation for removing numbers from list */
.fade-leave-active {
  position: absolute;
}

/* handles fading */
.fade-enter-from {
  opacity: 0;
}

.fade-enter-active {
  transition: all 1s linear;
}

.fade-leave-to {
  transition: all 1s linear;
  opacity: 0;
}

/* handles zoom */
h2 {
  width: 400px;
  padding: 20px;
  margin: 20px;
}

.zoom-enter-active {
  animation: zoom-in 1s linear forwards;
  transition: all 1s linear;
}

.zoom-leave-active {
  animation: zoom-out 1s linear forwards;
  transition: all 1s linear;
}

.zoom-enter-from {
  opacity: 0;
}

.zoom-leave-to {
  opacity: 0;
}

@keyframes zoom-in {
  from {
    transform: scale(0, 0);
  }
  to {
    transform: scale(1, 1);
  }
}

@keyframes zoom-out {
  from {
    transform: scale(1, 1);
  }
  to {
    transform: scale(0, 0);
  }
}
</style>
