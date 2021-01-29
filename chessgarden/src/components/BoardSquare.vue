<template>
  <g>
    <rect
      @mouseenter="handleEnter(message, $event)"
      @click="colorPixel"
      :x="computedXCoord"
      :y="computedYCoord"
      :width="sideLength"
      :height="sideLength"
      stroke="grey"
      stroke-width=".1"
      :fill="computedFill"
    />
    <svg
      :height="sideLength * 2.5"
      :width="sideLength * 2.5"
      :x="xCoord * sideLength - sideLength * 2.1 + sideLength"
      :y="yCoord * sideLength - sideLength + sideLength"
      :view-box.camel="viewbox"
    >
      <polygon
        id="Path"
        stroke="#0000FF"
        fill="#0000FF"
        points="40 320 40 300 60 300 60 290 70 290 70 280 80 280 80 90 60 90 60 80 40 80 40 30 100 30 100 60 130 60 130 30 180 30 180 60 220 60 220 30 280 30 280 80 260 80 260 90 240 90 240 280 250 280 250 290 260 290 260 300 280 300 280 320"
      ></polygon>
    </svg>
  </g>
</template>

<script>
export default {
  methods: {
    handleEnter(message, event) {
      if (event.buttons == 1) {
        this.colorPixelDrag();
      }
    },
    colorPixelDrag() {
      this.arrayPlaceHolder = this.pixMatrix;
      this.arrayPlaceHolder[this.yCoord][this.xCoord] = this.blueOrWhite;
      this.pixMatrix = this.arrayPlaceHolder.slice(0);
    },
    colorPixel() {
      if (this.pixMatrix[this.yCoord][this.xCoord] == "white") {
        this.arrayPlaceHolder = this.pixMatrix;
        this.arrayPlaceHolder[this.yCoord][this.xCoord] = "blue";
        this.pixMatrix = this.arrayPlaceHolder.slice(0);
      } else {
        this.arrayPlaceHolder = this.pixMatrix;
        this.arrayPlaceHolder[this.yCoord][this.xCoord] = "white";
        this.pixMatrix = this.arrayPlaceHolder.slice(0);
      }
    },
  },
  props: ["xCoord", "yCoord", "sideLength", "pixMatrix", "blueOrWhite"],
  data() {
    return {
      anothersideLength: 100,
      arrayPlaceHolder: [],
      vbx: 0,
      vby: 0,

      vbh: 984,
    };
  },
  computed: {
    viewbox() {
      const { vbx, vby, vbh } = this.$data;
      return [vbx, vby, this.sideLength, vbh].join(" ");
    },
    computedXCoord() {
      return this.xCoord * this.sideLength;
    },
    computedYCoord() {
      return this.yCoord * this.sideLength;
    },
    pixelAddress: function () {
      return [this.yCoord, this.xCoord];
    },

    computedFill() {
      return this.pixMatrix[this.yCoord][this.xCoord];
    },
  },
};
</script>