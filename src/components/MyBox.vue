<script>
/* eslint-disable no-console */
// Note how there's no template or styles in this component.

export default {
  // Gets us the provider property from the parent <my-canvas> component.
  inject: ['provider'],

  props: {
    // Start coordinates (percentage of canvas dimensions).
    x1: {
      type: Number,
      default: 0,
    },
    y1: {
      type: Number,
      default: 0,
    },

    // End coordinates (percentage of canvas dimensions).
    x2: {
      type: Number,
      default: 0,
    },
    y2: {
      type: Number,
      default: 0,
    },

    // The value to display.
    value: {
      type: Number,
      defualt: 0,
    },

    // The color of the box.
    color: {
      type: String,
      default: '#000',
    },
    rows: {
      type: Number,
      default: 0,
    },
    cols: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      // We cache the dimensions of the previous
      // render so that we can clear the area later.
      // oldBox: {
      //   x: null,
      //   y: null,
      //   w: null,
      //   h: null,
      // },
    };
  },
  computed: {
    calculatedBox() {
      // const ctx = this.provider.context;
      const largest = this.rows <= this.cols ? this.cols : this.rows;
      const size = Math.floor(this.provider.size / largest);
      // console.log(this.provider.size, largest, size);
      // Turn start / end percentages into x, y, width, height in pixels.
      const calculated = {
        x: this.x1 * size,
        y: this.y1 * size,
        w: (this.x2 - this.x1) * size,
        h: (this.y2 - this.y1) * size,
      };

      // Yes yes, side-effects. This lets us cache the box dimensions of the previous render.
      // before we re-calculate calculatedBox the next render.
      // eslint-disable-next-line vue/no-side-effects-in-computed-properties
      // this.oldBox = calculated;
      return calculated;
    },
  },
  render() {
    // Since the parent canvas has to mount first, it's *possible* that the context may not be
    // injected by the time this render function runs the first time.
    if (!this.provider.context) return;
    const ctx = this.provider.context;

    // Keep a reference to the box used in the previous render call.
    // const oldBox = this.oldBox;
    // Calculate the new box. (Computed properties update on-demand.)
    const newBox = this.calculatedBox;

    ctx.beginPath();
    // Clear the old area from the previous render.
    // ctx.clearRect(oldBox.x, oldBox.y, oldBox.w, oldBox.h);
    // Clear the area for the text.
    ctx.clearRect(newBox.x, newBox.y, newBox.w, newBox.h);

    // Draw the new rectangle.
    ctx.rect(newBox.x, newBox.y, newBox.w, newBox.h);
    ctx.fillStyle = this.color;
    ctx.fill();
    ctx.lineWidth = '2';
    ctx.strokeStyle = 'red';
    ctx.stroke();

    // Draw the text
    ctx.fillStyle = '#000';
    ctx.font = '28px sans-serif';
    ctx.textAlign = 'center';
    ctx.fillText(Math.floor(this.value), newBox.x + newBox.w / 2, newBox.y + newBox.h / 2);
    return true;
  },
};
</script>
