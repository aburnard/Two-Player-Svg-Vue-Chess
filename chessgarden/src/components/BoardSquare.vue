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
  </g>
</template>

<script>
export default {
  methods: {
    alert() {
      this.$buefy.dialog.alert("x is " + this.xCoord + "y is " + this.yCoord);
    },
  },
  props: ["xCoord", "yCoord"],
  data() {
    return {
      sideLength: 100,
    };
  },
  computed: {
    computedCoord() {
      return this.xCoord * 3;
    },
    computedXCoord() {
      return this.xCoord * this.sideLength;
    },
    computedYCoord() {
      return this.yCoord * this.sideLength;
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