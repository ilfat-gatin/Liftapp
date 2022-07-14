<template>
  <div class="shaft" v-bind:style="{ height: maxLevel * 100 + 'px'}">
    <div class="lift"
      v-bind:class="{ pause: pause }" 
      v-bind:style="{ bottom: pos + 'px' }" >
      <div class="directionField" v-if="directionField">
        <span v-if="direction">&#8593;</span>
        <span v-else>&#8595;</span>
        {{ targetLevel }}
      </div>
    </div>
  </div>
</template>

<script>


export default {
  data() {
    return {
      pos: 0,
      free: true,
      pause: false,
      targetLevel: 0,
      direction: false,
      directionField: false,
    }
  },
  props: {
    callArray: Array,
    maxLevel: Number
  },
  methods: {
    goTo(lvl) {
      return new Promise((response,reject) => {
        if (this.free) {
        if (lvl-1 === this.pos / 100) {
          response()
          return
        } 
        this.free = false
        this.directionField = true
        this.targetLevel = lvl
        let int = setInterval(() => {
          if (this.pos > (lvl-1) * 100) {
            this.direction = false
            this.pos -= 1
          } else if (this.pos < (lvl-1) * 100) {
            this.direction = true
            this.pos += 1
          }
          if (this.pos === (lvl-1) * 100) {
            clearInterval(int)           
            this.pause = true
            this.directionField = false
            setTimeout(() => {
              this.free = true
              this.pause = false
              response()
            }, 3000)
          }
        }, 10)
      }
      })
    },
    hadleCallArray() {
      let prm = this.goTo(this.callArray[0])   
      prm
      .then(() => {
        this.callArray.shift()
        if (this.callArray.length == 0) {
          return
        } else {
          this.hadleCallArray()
        }
      })
    }    
  }
}
</script>

<style>
.lift {
  height: 100px;
  background-color: grey;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
}

.pause {
  background-color: red;
  animation: blink 1s 3;
}

.directionField {
  width: 50px;
  background-color: aqua;
  text-align: center;
  margin: auto;
  border-radius: 5px;
}

.shaft {
  width: 100px;
  border: 1px solid black;
  position: relative;
  margin-right: 10px;
}

@keyframes blink {
  0% {
    background-color: grey;
  }
  50% {
    background-color: red;
  }
  100% {
    background-color: grey;
  }
}
</style>