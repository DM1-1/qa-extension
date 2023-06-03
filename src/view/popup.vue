<template>
  <div class="main_app">
    <h1 :class="{ red: isCountReached }">Hello! {{msg}}</h1>
    <div id="app">
      <button @click="count++">
        Count is: {{ count }}
      </button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'popupView',
  data () {
    return {
      msg: 'popup',
      count: 0
    }
  },
  methods: {
    incrementCount() {
      this.count ++
    }
  },
  computed: {
    isCountReached() {
      return this.count >= 10
    }
  }
}

</script>

<style>
.main_app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.red {
  background-color: red;
}
</style>
