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
    };
  },
  computed: {
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