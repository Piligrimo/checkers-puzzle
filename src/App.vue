<template>
  <div id="app">
    <password v-if="unauthorized" @check="checkAuth"/>
    <game  v-else/>
  </div>
</template>

<script>

import Game from './components/Game.vue'
import Password from './components/Password.vue'


export default {
  name: 'App',
  components: {
    Game,
    Password
  },
  data() {
    return {
      unauthorized: true
    }
  },
  mounted() {
    this.checkAuth()
  },
  methods: {
    checkAuth() {
      this.unauthorized = localStorage.getItem('password') !== '71016'
    }
  }

}
</script>

<style>
body {
  background-color: #f1e6d1;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
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
  box-shadow: 0px 0px 8px 8px rgb(59, 242, 102);
}

.available {
  display: block;
}

</style>
