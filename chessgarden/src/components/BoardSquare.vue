<template>
  <g>
    <circle
      @click="alert"
      cx="300"
      cy="1000"
      r="40"
      stroke="green"
      stroke-width="4"
      fill="red"
    />
    <rect
      @click="alert"
      :x="computedXCoord"
      :y="computedYCoord"
      :width="sideLength"
      :height="sideLength"
      stroke="green"
      stroke-width="8"
      fill="red"
    />
  </g>
</template>

<script>
export default {
  methods: {
    alert() {
      this.$buefy.dialog.alert(
        "x is " + this.xCoord + "ycor is " + this.computedYCoord
      );
    },
  },
  props: ["xCoord"],
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
      return (
        this.xCoord * this.sideLength -
        Math.floor(this.xCoord / 8) * this.sideLength * 8
      );
    },
    computedYCoord() {
      return Math.floor(this.xCoord / 8) * this.sideLength;
    },
  },
};
</script>