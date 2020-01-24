<template>
  <div id="app" @click="handleClick">
    <!-- These are the custom components we'll create -->
    <!-- Values for `my-box` are percentages of the width of the canvas. -->
    <!-- Each bar will take up an equal space of the canvas. -->
    <my-canvas style="width: 100%; height: 100%;">
      <my-box
        v-for="(obj, index) of grid"
        :key="index"
        :rows="rows"
        :cols="cols"
        :x1="obj.row"
        :x2="obj.row + 1"
        :y1="obj.col"
        :y2="obj.col + 1"
        :color="obj.color"
        :value="obj.val"
      >
      </my-box>
    </my-canvas>
  </div>
</template>

<script>
import MyCanvas from './components/MyCanvas.vue';
import MyBox from './components/MyBox.vue';

export default {
  name: 'app',
  components: {
    MyCanvas,
    MyBox,
  },
  data() {
    return {
      rows: 5,
      cols: 5,
      grid: [],
      colors: ['red', 'green', 'blue', 'orange', 'purple'],
    };
  },
  methods: {
    handleClick() {
      // eslint-disable-next-line no-console
      console.log('click');
    },
  },
  // Randomly selects a value to randomly increment or decrement every 16 ms.
  // Not really important, just demonstrates that reactivity still works.
  mounted() {
    // function step(timestamp) {
    //   if (!start) start = timestamp;
    //   var progress = timestamp - start;
    //   element.style.transform = 'translateX(' + Math.min(progress / 10, 200) + 'px)';
    //   if (progress < 2000) {
    //     window.requestAnimationFrame(step);
    //   }
    // }

    // window.requestAnimationFrame(step);

    for (let i = 0; i < this.rows; i += 1) {
      for (let j = 0; j < this.cols; j += 1) {
        this.grid.push({
          val: i * j,
          row: i,
          col: j,
          // color: this.colors[i],
        });
      }
    }

    // eslint-disable-next-line no-console

    // let dir = 1;
    // let selectedVal = Math.floor(Math.random() * this.chart.length);

    // setInterval(() => {
    //   if (Math.random() > 0.995) dir *= -1;
    //   if (Math.random() > 0.99) selectedVal = Math.floor(Math.random() * this.chart.length);

    //   this.chart[selectedVal].val = Math.min(Math.max(this.chart[selectedVal].val + dir * 0.5, 0), 100);
    // }, 16);
  },
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: relative;
  height: 100vh;
  width: 100vw;
  box-sizing: border-box;
}
</style>
