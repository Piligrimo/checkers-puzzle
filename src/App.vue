<template>
  <div id="app">
    <threat v-if="isAuth"/>
    <password v-else-if="secret" @check="checkAuth"/>
    <game @secret="secret = true"  v-else/>
  </div>
</template>

<script>

import Game from './components/Game.vue'
import Password from './components/Password.vue'
import Threat from './components/Threat.vue'


export default {
  name: 'App',
  components: {
    Game,
    Password,
    Threat
  },
  data() {
    return {
      secret: false,
      isAuth: false
    }
  },
  mounted() {
    this.checkAuth()
  },
  methods: {
    checkAuth(password) {
      if (password === '71016')
        this.isAuth = true
    }
  }

}
</script>

<style>
html {
  height: 100%;
}
body {
  background-color: #f1e6d1;
  width: 100%;
  height: 100%;
  margin: 0;
}
.game{
  width: 95%;
  margin: auto;
}
#app {
  height: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #3b1b08;
  margin-top: 20px;
}

#button {
  background-image: url('./assets/фон кнопки.png');
  font-size: 40px;
  color: #5d3d1b;
  border: #5a3616 3px solid;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0px 5px 5px 0px rgba(0,0,0,0.49);

}

#button:active {
  filter: brightness(0.8);
  transform: translateY(5px);
  box-shadow: 0px 1px 5px 0px rgba(0,0,0,0.49);

}

#container {
  display: grid;
  border: #724722 3px solid;
  border-radius: 10px;
  grid-gap: 0;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
  background-image: url('./assets/фон.png');
  background-repeat: no-repeat;
  background-size: cover;
  box-shadow: 0px 10px 25px 0px rgba(0,0,0,0.49);
}

.square {
  display: flex;
  justify-content: center;
  align-items: flex-end;
  padding-bottom: 25%;
}

.checker {
  width: 90%;
  border-radius: 100%;
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
  box-shadow: 0px 0px 8px 8px rgb(59, 242, 102);
}

.available {
  display: block;
}

</style>
