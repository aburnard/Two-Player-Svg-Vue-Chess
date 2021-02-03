<template>
  <g>
    <rect
      @click="alert"
      :x="computedXCoord"
      :y="computedYCoord"
      :width="sideLength"
      :height="sideLength"
      stroke="brown"
      stroke-width="5"
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
        :points="chessset.chessset[chessset.board[yCoord][xCoord]]"
      ></polygon>
    </svg>
  </g>
</template>

<script>
export default {
  methods: {
    alert() {
      // this.$buefy.dialog.alert("x is " + this.xCoord + "y is " + this.yCoord);
      //  this.$emit("emitPixel", this.pixelAddress);
      this.$emit("emitPixel", { xOne: this.xCoord + 1, yOne: this.yCoord + 1 });
    },
  },
  props: ["xCoord", "yCoord", "sideLength", "chessset"],
  data() {
    return {
      anothersideLength: 100,
      vbx: 0,
      vby: 0,
      vbh: 1500,
    };
  },
  computed: {
    path() {
      return this.chessset;
    },
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
      if (
        (this.xCoord % 2 && this.yCoord % 2) ||
        (this.xCoord % 2 == 0 && this.yCoord % 2 !== 1)
      ) {
        return "white";
      } else return "black";
    },
  },
};
</script>