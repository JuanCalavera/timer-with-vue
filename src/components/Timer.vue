<template>
  <section class="container">
    <div class="base-timer">
      <input type="text" class="title-timer" v-model="titleTimer" />
      <h2 v-show="count">{{ hours }}:{{ minutes }}:{{ seconds }}</h2>
      <div class="d-flex" v-show="!count">
        <input
          type="number"
          min="1"
          class="input-time"
          v-model="hours"
          placeholder="HH"
        />
        <input
          type="number"
          min="1"
          class="input-time"
          v-model="minutes"
          placeholder="MM"
        />
        <input
          type="number"
          min="1"
          class="input-time"
          v-model="seconds"
          placeholder="SS"
        />
      </div>
      <div @click="initTime">
        <svg
          v-if="play"
          xmlns="http://www.w3.org/2000/svg"
          width="160"
          height="160"
          fill="currentColor"
          class="bi bi-play-fill"
          viewBox="0 0 16 16"
        >
          <path
            d="m11.596 8.697-6.363 3.692c-.54.313-1.233-.066-1.233-.697V4.308c0-.63.692-1.01 1.233-.696l6.363 3.692a.802.802 0 0 1 0 1.393z"
          />
        </svg>

        <svg
          v-else
          xmlns="http://www.w3.org/2000/svg"
          width="160"
          height="160"
          fill="currentColor"
          class="bi bi-pause"
          viewBox="0 0 16 16"
        >
          <path
            d="M6 3.5a.5.5 0 0 1 .5.5v8a.5.5 0 0 1-1 0V4a.5.5 0 0 1 .5-.5zm4 0a.5.5 0 0 1 .5.5v8a.5.5 0 0 1-1 0V4a.5.5 0 0 1 .5-.5z"
          />
        </svg>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      play: true,
      finish: false,
      count: false,
      hours: "HH",
      minutes: "MM",
      seconds: "SS",
      titleTimer: "timer 1",
      interval: () => {},
    };
  },
  methods: {
    verifyInput(hour, minutes) {
      if (minutes > 0 || hour > 0) {
        this.play = !this.play;
        this.count = !this.count;
      }
    },

    rewrite(n) {
      if (n < 10) {
        n = `0${n}`;
      }
      if (n === "HH" || n === "MM" || n === "SS") {
        n = "00";
      }
      return n;
    },

    initTime() {
    this.verifyInput(this.hours, this.minutes);
      if (!this.play) {
        this.hours = this.rewrite(this.hours);
        this.minutes = this.rewrite(this.minutes);
        this.interval = setInterval(() => {
          if (this.minutes == 0) {
            this.minutes = 59;
            this.hours--;
            this.hours = this.rewrite(this.hours);
          } else if (this.seconds != 0) {
            this.seconds--;
            this.seconds = this.rewrite(this.seconds);
          } else if (this.minutes == 0 && this.hours == 0) {
            this.finish = !this.finish;
            this.play = !this.play;
            clearInterval(this.interval);
          } else if (this.seconds == 0) {
            this.seconds = 59;
            this.minutes--;
            this.minutes = this.rewrite(this.minutes);
          }
        }, 1000);
      } else if (this.play) {
        clearInterval(this.interval);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
h2 {
  font-size: 48px;
}

.title-timer {
  background-color: #676767;
  color: #fff;
  font-style: bold;
  border: none;
  margin: 2px;
  padding: 10px;
  text-align: center;
  font-size: 30px;
}

.title-timer:focus {
  background-color: #b3b3b3;
  color: #fff;
  font-style: bold;
  border: none;
  margin: 2px;
  padding: 10px;
  resize: horizontal;
  widows: 100%;
}

.container {
  margin-left: 30%;
  margin-right: 30%;
}
.base-timer {
  padding: 30px;
  border-radius: 30px;
  background-color: #676767;
  color: white;
}
.d-flex {
  display: flex;
  justify-content: center;
}

.input-time {
  background-color: #b3b3b3;
  color: #fff;
  font-style: bold;
  border: none;
  margin: 2px;
  padding: 10px;
  resize: horizontal;
  width: 20px;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type="number"] {
  -moz-appearance: textfield;
}
</style>