<template>
  <div class="main">
    <MainButton
      :time="convertTime(t1)"
      @click="initTimer('1')"
      :class="[t1active ? 'active' : '']"
    />
    <div class="keypad">
      <button :disabled="state === 'stop' ? true : false" @click="toggle">pause</button>
      <button :disabled="state === 'play' ? true : false" @click="reset">reset</button>
      <button :disabled="state === 'play' ? true : false" @click="showConfig = true">config</button>
    </div>
    <MainButton
      :time="convertTime(t2)"
      @click="initTimer('2')"
      :class="[t2active ? 'active' : '']"
    />
  </div>
  <div v-show="showConfig" class="modal">
    <div class="modal-content">
      <select v-model="time">
        <option value="0.5">1/2</option>
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
        <option value="6">6</option>
        <option value="7">7</option>
        <option value="8">8</option>
        <option value="9">9</option>
        <option value="10">10</option>
        <option value="15">15</option>
        <option value="20">20</option>
        <option value="25">25</option>
        <option value="30">30</option>
      </select>
      <button @click="setTime">set time</button>
    </div>
  </div>
</template>

<script>
import MainButton from "./components/MainButton.vue";

export default {
  data() {
    return {
      timer: null,
      state: "stop",
      index: "0",
      t1: 60000,
      t2: 60000,
      t1active: false,
      t2active: false,
      showConfig: false,
      time: 60000
    };
  },
  methods: {
    initTimer(p) {
      if (this.state === "stop") this.state = "play";
      if (this.t1 > 0 && this.t2 > 0) {
        if (p === "1") {
          this.index = "1";
          clearInterval(this.timer);
          this.switchActive("1");
          this.timer = setInterval(() => {
            this.t2 -= 10;
            this.checkTime();
          }, 10);
        } else {
          this.index = "2";
          clearInterval(this.timer);
          this.switchActive("2");
          this.timer = setInterval(() => {
            this.t1 -= 10;
            this.checkTime();
          }, 10);
        }
      }
    },
    convertTime(t) {
      let milliseconds = Math.floor((t % 1000) / 100);
      let seconds = Math.floor((t / 1000) % 60);
      let minutes = Math.floor((t / (1000 * 60)) % 60);
      minutes = minutes < 10 ? "0" + minutes : minutes;
      seconds = seconds < 10 ? "0" + seconds : seconds;
      let extra = minutes <= 0 ? "." + milliseconds : "";
      return minutes + ":" + seconds + extra;
    },
    switchActive(i) {
      if (i === "1") {
        this.t1active = true;
        this.t2active = false;
      } else {
        this.t1active = false;
        this.t2active = true;
      }
    },
    checkTime() {
      if (this.t1 <= 0 || this.t2 <= 0) {
        clearInterval(this.timer);
        this.timer = null
        this.state = "stop"
        this.index = "0"
        this.t1 = this.time
        this.t2 = this.time
        this.t1active = true
        this.t2active = true
        return true;
      } else return false;
    },
    toggle() {
      if (this.state === "play") {
        clearInterval(this.timer);
        this.t1active = true;
        this.t2active = true;
        this.timer = null;
        this.state = "paused";
      } else if (this.state === "paused") {
        this.initTimer(this.index);
        this.state = "play";
      }
    },
    reset(){
      clearInterval(this.timer);
      this.timer = null
      this.state = "stop"
      this.index = "0"
      this.t1 = this.time
      this.t2 = this.time
      this.t1active = false
      this.t2active = false
    },
    setTime(){
      this.time = parseFloat(this.time) * 60000
      this.reset()
      this.showConfig = false
    }
  },
  components: { MainButton },
  mounted() {
    this.t1 = this.time
    this.t2 = this.time
  }
};
</script>

<style>
.main {
  width: 100vw;
  height: 100vh;
  background-color: #2a2a2a;
  padding: 1em;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.keypad {
  height: 20%;
  width: 100%;
  display: flex;
  gap: 1em;
  justify-content: center;
  align-items: center;
}
.keypad button {
    height: 80%;
    width: 30%;
}
.invert {
  transform: scale(-1, -1);
  -moz-transform: scale(-1, -1);
  -webkit-transform: scale(-1, -1);
  -o-transform: scale(-1, -1);
  -ms-transform: scale(-1, -1);
  transform: scale(-1, -1);
}
.active {
  background-color: gray;
}
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content {
  background-color: #353535;
  width: 90vw;
  height: 30vh;
  padding: 1em;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  gap: .5em;
}
.modal-content select {
  height: 40px;
  width: 100%;
}
.modal-content button {
  height: 40px;
  width: 100%;
}
</style>
