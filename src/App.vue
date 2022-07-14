<template>
  <div class="app"> 
      <Lift :maxLevel="levels[length - 1]" :callArray="liftArrs.arr1" ref="ref1"/>
      <Lift :callArray="liftArrs.arr2" ref="ref2"/>
      <Lift :callArray="liftArrs.arr3" ref="ref3"/>
      <div class="btns_wrapper">
        <div class="btn_div" v-for="(level, index) in levels" v-bind:key="index">
          <button v-bind:class="{ button_call : btnCall}" class="button" @click="handle(level)">{{ level }}</button>
        </div>
      </div>   
  </div>
</template>

<script>
import Lift from "./components/Lift.vue"

export default {
  
  data() {
    return {
      liftCalls: [],
      levels: [1,2,3,4,5,6,7], 
      liftArrs: {
        arr1: [],
        arr2: [],
        arr3: []
      },
      liftCount: 3,
      btnCall: false
    }
  },
  components: {
    Lift
  },
  methods: {
    start(ref) { 
      this.$refs["ref" + ref].hadleCallArray()
      this.$refs["ref" + ref].hadleCallArray()
      this.$refs["ref" + ref].hadleCallArray()
    },
    checkForFreeLift() {
      for (let i = 1; i <= this.liftCount; i++) {
        if (this.$refs["ref" + i].free) {
          return true
        }
      }
      return false
    },
    handle(lvl) {
      let min = 20
      let index = null
      for (let i = 1; i <= this.liftCount; i++) {
          if (this.$refs["ref" + i].free === true) {
            this.liftArrs["arr" + i].push(lvl)
            this.start(i)
            return
          }
          if (!this.checkForFreeLift() && Math.abs(this.$refs["ref" + i].targetLevel - lvl) < min) {
            min = Math.abs(this.$refs["ref" + i].targetLevel - lvl)
            index = i
          }
      }
      this.liftArrs["arr" + index ].push(lvl)
    },   
  },
}


</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 15px;
  display: flex;
}

.btns_wrapper{
  display: flex;
  flex-direction: column-reverse;
}

.btn_div {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;  
}

.button {
  padding: 5px;
  background-color: transparent;
  cursor: pointer;
  border-radius: 15px;
}
.button:hover {
  background-color: lightgrey;
}
.button_call {
  background-color: orange;
}
</style>
