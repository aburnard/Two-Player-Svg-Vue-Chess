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
      stroke-width="1"
      :fill="computedFill"
    />
  </g>
</template>

<script>
export default {
  methods: {
    handleEnter(message, event) {
      if (event.buttons == 1) {
        this.colorPixel();
      }
    },
    colorPixel() {
      //this.pixMatrix.splice([this.yCoord][this.xCoord], 2, "blue");

      // this.pixMatrix[this.yCoord][this.xCoord] = "blue";
      this.arrayPlaceHolder = this.pixMatrix;
      this.arrayPlaceHolder[this.yCoord][this.xCoord] = "blue";
      this.pixMatrix = this.arrayPlaceHolder.slice(0);

      //  this.pixMatrix[this.yCoord][this.xCoord];
      // if (this.computedFill == "white") {
      //   this.computedFill = "black";
      // }
    },
  },
  props: ["xCoord", "yCoord", "sideLength", "pixMatrix"],
  data() {
    return {
      anothersideLength: 100,
      arrayPlaceHolder: [],
      //  computedFill: "white",
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
      // return "blue";
    },
  },
};
</script>