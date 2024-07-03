<template>
  <div class="circle-progress">
    <svg viewBox="0 0 36 36" class="circular-chart" :class="progressColor">
      <path class="circle-bg"
        d="M18 2.0845
          a 15.9155 15.9155 0 0 1 0 31.831
          a 15.9155 15.9155 0 0 1 0 -31.831" />
      <path class="circle"
        :stroke-dasharray="progressValue + ', 100'"
        d="M18 2.0845
          a 15.9155 15.9155 0 0 1 0 31.831
          a 15.9155 15.9155 0 0 1 0 -31.831" />
    </svg>
    <div class="text">
      <div v-if="Value < TargetValue">Ещё продать на завтра</div>
      <div>{{ Value < TargetValue ? remainingHours + ' ч' : Value + ' ч' }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CircleProgress',
  props: {
    MaxValue: {
      type: Number,
      required: true
    },
    Value: {
      type: Number,
      required: true
    },
    TargetValue: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      animatedValue: 0
    };
  },
  computed: {
    remainingHours() {
      return this.TargetValue - this.Value;
    },
    progressValue() {
      return (this.animatedValue / this.MaxValue) * 100;
    },
    progressColor() {
      return this.Value >= this.TargetValue ? 'green' : 'orange';
    }
  },
  mounted() {
    this.animateProgress();
  },
  methods: {
    animateProgress() {
      let start = 0;
      const end = this.Value;
      const duration = 1000; // duration in ms
      const stepTime = Math.abs(Math.floor(duration / end));
      const timer = setInterval(() => {
        start += 1;
        this.animatedValue = start;
        if (start >= end) {
          clearInterval(timer);
        }
      }, stepTime);
    }
  }
}
</script>

<style scoped>
.circle-progress {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.circular-chart {
  width: 100%;
  max-width: 200px;
}
.circle-bg {
  fill: none;
  stroke: #eee;
  stroke-width: 3.8;
}
.circle {
  fill: none;
  stroke-width: 2.8;
  stroke-linecap: round;
  animation: progress 1s ease-out forwards;
}
.orange .circle {
  stroke: orange;
}
.green .circle {
  stroke: green;
}
.text {
  font-size: 1.2rem;
  text-align: center;
  margin-top: -100px;
}
@keyframes progress {
  0% {
    stroke-dasharray: 0, 100;
  }
}
</style>
