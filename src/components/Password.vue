<template>
  <div class="password">
    <input class="input" :class="{shakin}" v-model="password"  placeholder="Пароль">
    <button id="button" @click="login">Войти</button>
  </div>
</template>

<script>
export default {

  name: 'App',
  components: {
  },
  data() {
    return {
      password: '',
      shakin: false
    }
  },
  methods: {
    login() {
      localStorage.setItem('password', this.password)
      this.$emit('check')
      this.shakin = true 
      const vue = this
      setTimeout(() => {vue.shakin = false}, 1000)
    }
  }
  
}
</script>

<style>
.password {
  display: flex;
  flex-direction: column;
  max-width: 350px;
  margin: auto;
}
.input {
  margin-top: 50px;
  margin-bottom: 50px;
  font-size: 30px;
  padding: 15px;
  border: #724722 3px solid;
  border-radius: 10px;
}

.shakin {
  animation-name: shake;
  animation-duration: .2s;
  animation-timing-function: linear;
}

@keyframes shake {
  0% {
      transform: translate(15px,10px);
  }
  20% {
      transform: translate(-30px,0);
  }
  40% {
      transform: translate(10px,-15px);
  }
  60% {
      transform: translate(-14px,-1px);
  }
  80% {
      transform: translate(16px,-8px);
  }
  100% {
    transform: translate(0,0); 
  }
}
</style>
