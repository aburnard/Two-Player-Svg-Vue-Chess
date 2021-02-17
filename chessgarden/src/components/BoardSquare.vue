<template>
  <g>
    <rect
      @click="alert"
      :x="computedXCoord"
      :y="computedYCoord"
      :width="sideLength"
      :height="sideLength"
      stroke="purple"
      :stroke-width="selectedSquareStroke"
      :fill="computedFill"
    />
    <svg
      @click="alert"
      :height="sideLength * 2"
      :width="sideLength * 2"
      :x="xCoord * sideLength - sideLength * 1.8 + sideLength"
      :y="yCoord * sideLength - sideLength + sideLength"
      :view-box.camel="viewbox"
    >
      <polygon
        id="Path"
        stroke="#47473b"
        :fill="returnPieceColorAtSquare"
        :points="returnPiecePathAtSquare"
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
  props: [
    "chessJsBoard",
    "xCoord",
    "yCoord",
    "sideLength",
    "chessset",
    "selectedSquare",
  ],
  data() {
    return {
      anothersideLength: 100,
      vbx: this.sideLength * -1.5,
      vby: this.sideLength * -1.5,
      vbh: this.sideLength * 25,
      wbToColor: { w: "white", b: "#47473b" },
    };
  },
  computed: {
    returnPiecePathAtSquare() {
      if (this.chessJsBoard[this.yCoord][this.xCoord]) {
        return this.chessset.piecepaths[
          this.chessJsBoard[this.yCoord][this.xCoord].type
        ];
      } else return this.chessset.piecepaths.x;
    },
    returnPieceColorAtSquare() {
      if (this.chessJsBoard[this.yCoord][this.xCoord]) {
        return this.wbToColor[
          this.chessJsBoard[this.yCoord][this.xCoord].color
        ];
      } else return this.chessset.piecepaths.x;
    },
    pieceColor() {
      return "#47473b";
    },

    selectedSquareStroke() {
      if (
        this.selectedSquare.pieceX - 1 == this.xCoord &&
        this.selectedSquare.pieceY - 1 == this.yCoord
      ) {
        return 6;
      } else {
        return 1;
      }
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
        return "yellow";
      } else return "red";
    },
  },
};
</script>