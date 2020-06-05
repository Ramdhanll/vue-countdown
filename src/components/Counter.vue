<template>
  <div v-if="loaded">
    <section class="text-3xl flex justify-center content-center flex-col mx-auto text-center">
      <button style="width: 200px" v-if="!expired" class="m-auto bg-yellow-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Buy Now</button>
      <h5 v-else>Timer Is Done</h5>
    </section>
      <section class="flex text-6xl justify-center content-center">
        <div class="days mr-2 relative">
          {{ displayDays }}
          <div class="label text-sm absolute bottom-0">Days</div>
        </div>
        <span class="leading-snug">:</span>
        <div class="hours mx-2 relative">
          {{ displayHours }}
          <div class="label text-sm absolute bottom-0">Hours</div>
        </div>
        <span class="leading-snug">:</span>
        <div class="minutes mx-2 relative">
          {{ displayMinutes }}
          <div class="label text-sm absolute bottom-0">Minutes</div>
        </div>
        <span class="leading-snug">:</span>
        <div class="seconds ml-2 relative">
          {{ displaySeconds }}
          <div class="label text-sm absolute bottom-0">Seconds</div>
        </div>
      </section>
  </div>
</template>

<script>
export default {
  props: ['year', 'month', 'date', 'hour','minutes', 'second','milisecond'],
  data () {
    return {
      displayDays: 0,
      displayHours: 0,
      displayMinutes: 0,
      displaySeconds: 0,
      loaded: false,
      expired: false
    }
  },
  computed: {
    _seconds: () => 1000, // 1s = 1000ms
    _minutes(){
      return this._seconds * 60;
    },
    _hours() {
      return this._minutes * 60;
    },
    _days(){
      return this._hours * 24;
    },
    end() {
      return new Date(
        this.year,
        this.month,
        this.date,
        this.hour,
        this.minutes,
        this.second,
        this.milisecond
      )
    }
  },
  mounted() {
    this.showRemaining();
  },
  methods: {
    formatNum:(num) => (num < 10 ? `0${num}` : num),
    showRemaining(){
      const timer = setInterval(() => {
        const now = new Date();
        // const end = new Date(2020, 5, 5, 23, 0, 0);
        const distance = this.end.getTime() - now.getTime();

        if (distance < 0 ) {
          clearInterval(timer);
          this.expired = true;
          return
        }

        const days = Math.floor((distance / this._days));
        const hours = Math.floor((distance % this._days) / this._hours);
        const minutes = Math.floor((distance % this._hours) / this._minutes);
        const seconds = Math.floor((distance % this._minutes) / this._seconds);

        this.displayMinutes = this.formatNum(minutes);
        this.displaySeconds = this.formatNum(seconds);
        this.displayHours = this.formatNum(hours);
        this.displayDays = this.formatNum(days);
        this.loaded = true;
      }, 1000)
    }
  },
}
</script>

<style>

</style>