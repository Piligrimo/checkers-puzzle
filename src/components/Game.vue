<template>
  <div class="game">
    <div id="container" ref="container" :style="`height: ${boardHeight}px`">
      <div class="square" v-for="(checker, i) in checkers" :key="i">
        <img 
          class="checker" 
          :src="getTexture(checker.type)"
          :class="[
            checker.type,
            chosenChecker === i ? 'chosen' : '',
            avalableSquare === i ? 'available' : '' 
          ]"
          @click="handleClick(i)"
        />
      </div>
    </div>
    <template v-if="winningCondition">
      <h3>
        Время не тратя иди напрямик <br>
        В место, где старый стоит грузовик. <br>
        Там возле белых живых голубей <br>
        Круг образует россыпь камней. <br>
        Выберешь камень и можешь бежать. <br>
      </h3>

      <h3>
        P.S.:Сердце подскажет, какой нужно брать.
      </h3>

      <h3>
        Камень доставить нужно быстрей<br>
        В место, где много странных вещей.<br>
        Русский поэт имя улице дал.<br>
        В арке скрывается нужный «квартал»<br>
        Арка в «квартал» приведёт прямиком<br>
        Там отыщи то ли гриб, то ли дом.<br>
      </h3>
      <br><br>
    </template>
    <h1 v-else >{{ message }}</h1>
    <button id="button" @click="restart">Рестарт</button>
  </div>
</template>

<script>
import blackTexture from '../assets/b.png'
import whiteTexture from '../assets/w.png'
import availableTexture from '../assets/a.png'
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
  name: 'TheGame',
  props: {
    msg: String
  },
  data() {
    return {
      checkers: initialPosition.slice(),
      chosenChecker: null,
      avalableSquare: null,
      moveCount: 15,
      boardHeight: 200,
      restartCount: 0
    };
  },
  created() {
    window.addEventListener("resize", this.handleResize);
  },
  mounted() {
    this.handleResize()
  },
  destroyed() {
    window.removeEventListener("resize", this.handleResize);
  },
  computed: {
    avalableMoves() {
      return this.checkers.map((item, i) => this.avalableMove(item?.type, i))
    },
    isThereMoves() {
      return this.avalableMoves.some((item) => item !== null)
    },
    message() {
      if (this.winningCondition) return "Ура, победа!!!" 
      if (this.moveCount<0) return "Кончились ходы!" 
      if (!this.isThereMoves) return "Нет доступных ходов!"
      return this.moveCount
    },
    winningCondition() {
      const winningPosition = [
        {type: 'b'},
        {type: 'b'},
        {type: 'b'},
        {type: 'e'},
        {type: 'w'},
        {type: 'w'},
        {type: 'w'}
      ]
      return JSON.stringify(this.checkers) === JSON.stringify(winningPosition)
    }
  },
  methods: {
    handleResize() {
      this.boardHeight = this.$refs.container?.clientWidth * 0.54
    },
    getTexture(type) {
      if (type === 'w') return whiteTexture
      if (type === 'b') return blackTexture
      return availableTexture
    },
    restart() {
      this.checkers =  initialPosition.slice()
      this.chosenChecker =  null
      this.avalableSquare =  null
      this.moveCount = 15
      this.restartCount ++
      if (this.restartCount>10)
        this.$emit('secret')
    },
    handleClick(i) {
      this.restartCount = 0
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
          this.moveCount--
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
