<template>
  <div class="threat">
    <h1 v-if="time < 12" class="next-location">Следующая локация{{ dots }}</h1>
    <template v-else>
      <div class="glitches"></div>
      <h1 class="red-text">Наш информатор из полиции сообщил, что Стрекоза арестован, а ты не тот человек,  за которого себя выдаёшь.</h1>
      <br/>
      <h1 class="red-text">Мы приняли меры. Проверь телефон!</h1>
    </template>
  </div>
</template>

<script>

export default {

  name: 'TheThreat',
  components: {
  },
  data() {
    return {
      time: 1
    }
  },
  created() {
    const vue = this
    setInterval(() =>  {
      vue.time ++
    }, 500)
  },
  computed: {
    dots() {
      return '.'.repeat(this.time % 3 + 1)
    }
  }
}
</script>

<style>
.threat {
  width: 100%;
  height: 100%;
  padding-top: 100px;
}
.next-location {
  width: 380px;
  margin: auto;
  text-align: left;
}
.red-text {
  color: red;
  width: 90%;
  margin: auto;
  z-index: 3;
}

.glitches {
  width: 100%;
  height: 100%;
  background-image: url('../assets/glitches.gif');
  background-size: cover;
  position: fixed;
  top: 0;
  opacity: 0.4;
}
</style>
