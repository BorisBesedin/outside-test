<template>
  <div id="app">
    <button class="app__button" @click="popupIsOpened = true">Налоговый вычет</button>

    <div class="overlay" v-if="popupIsOpened">
      <Popup v-if="popupIsOpened" @close-popup="popupIsOpened = false" />
    </div>
  </div>
</template>

<script>
import Popup from './components/Popup'
export default {
  data() {
    return {
      popupIsOpened: false
    }
  },
  name: 'App',
  components: {
    Popup
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

ul {
  list-style: none;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  color: #000000;
  width: 100vw;
  min-height: 100vh;
  background: linear-gradient(to right,#FF5E56, #DC3131);
}

.app__button {
  color: #ffffff;
  padding: 16px 32px;
  background: linear-gradient(to right,#FF5E56, #DC3131);
  border: 2px solid #ffffff;
  border-radius: 5px;
  cursor: pointer;
  outline: none;
  font-size: 16px;
  line-height: 24px;
  font-weight: bold;
}
.app__button:hover {
  background: #ffffff;
  color: #000000;  
}

.overlay {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: #a8a8a8;
  z-index: 5px;
  overflow-y: scroll;
}
</style>
