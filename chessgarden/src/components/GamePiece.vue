<template>
  <g>
    <circle
      @click="moveit"
      :cx="xCoord * sideLength - sideLength / 2"
      :cy="yCoord * sideLength - sideLength / 2"
      :r="sideLength * 0.4"
      stroke="black"
      stroke-width="3"
      fill="red"
    />
  </g>
</template>

<script>
export default {
  methods: {
    alert() {
      this.$buefy.dialog.alert("x is " + this.xCoord + "y is " + this.yCoord);
    },
    moveit(x, y) {
      this.aCoord = x * this.sideLength;
      this.bCoord = y * this.sideLength;

      //this.anothersideLength += 50;
    },
  },
  props: ["xCoord", "yCoord", "sideLength", "pieceX", "pieceY"],
  data() {
    return {
      aCoord: 300,
      bCoord: 300,
      anothersideLength: 100,
    };
  },
  computed: {
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