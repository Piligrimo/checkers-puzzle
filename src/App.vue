<template>
  <div id="app">
    <div class="container">
      <div class="square" v-for="(checker, i) in checkers" :key="i">
        <div 
          class="checker" 
          :class="[
            checker.type,
            chosenChecker === i ? 'chosen' : '',
            avalableSquare === i ? 'available' : '' 
          ]"
          @click="handleClick(i)"
        >
        </div>
      </div>
    </div>
    <p v-if="!isThereMoves">Нет ходов!!</p>
    <p >{{ moveCount }}</p>
    <button @click="restart">рестарт</button>
  </div>
</template>

<script>
const initialPosition =  [
        {type: 'w'},
        {type: 'w'},
        {type: 'w'},
        {type: 'e'},
        {type: 'b'},
        {type: 'b'},
        {type: 'b'},
      ]

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      checkers: initialPosition.slice(),
      chosenChecker: null,
      avalableSquare: null,
      moveCount:0 ,
    };
  },
  computed: {
    avalableMoves() {
      return this.checkers.map((item, i) => this.avalableMove(item?.type, i))
    },
    isThereMoves() {
      return this.avalableMoves.some((item) => item !== null)
    }
  },
  methods: {
    restart() {
      this.checkers =  initialPosition.slice()
      this.chosenChecker =  null
      this.avalableSquare =  null
      this.moveCount = 0 
    },
    handleClick(i) {
      if (!this.isThereMoves) return
      const { type } = this.checkers[i]
      if (type !== 'e' && i !== this.chosenChecker) {
        this.chosenChecker = i
        this.avalableSquare = this.avalableMove(type, i)
      } else {
        if (type === 'e') {
          const newCheckers= this.checkers.slice()
          this.$set(this.checkers, i, { type:  newCheckers[this.chosenChecker].type })
          this.$set(this.checkers,this.chosenChecker, { type:'e'})
          this.moveCount++
        }
        this.chosenChecker = null
        this.avalableSquare = null
      }
    },
    avalableMove(type, position) {
      if (type === 'w') {
        if (this.checkers[position + 1]?.type === 'e') return position + 1
        if (this.checkers[position + 2]?.type === 'e') return position + 2
      }
      if (type === 'b') {
        if (this.checkers[position - 1]?.type === 'e') return position - 1
        if (this.checkers[position - 2]?.type === 'e') return position - 2
      }
      return null
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  display: flex;
  flex-direction: row;
}

.square {
  height: 200px;
  width: 200px;
  border: #3a230a 1px solid;
  background-color: darkgoldenrod;
  display: flex;
  justify-content: center;
  align-items: center;
}

.checker {
  height: 100px;
  width: 100px;
  border: #3a230a 1px solid;
  border-radius: 50px;
  cursor: pointer;
}

.w {
  background-color: #e7c5a0;
}

.b {
  background-color: #5d3d1b;
}

.e {
  display: none;
}

.chosen {
  box-shadow: 0px 0px 10px 10px rgb(59, 242, 102);
}

.available {
  display: block;
  background-color:  rgb(59, 242, 102);
  border: none;
  box-shadow: 0px 0px 10px 10px rgb(59, 242, 102);
  height: 50px;
  width: 50px;
}

</style>
