<template>
  <g>
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
  },
};
</script>