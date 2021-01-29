<template>
  <g>
    <g>
      <svg
        :height="sideLength * 2.5"
        :width="sideLength * 2.5"
        :x="xCoord * sideLength - sideLength * 2.1"
        :y="yCoord * sideLength - sideLength"
        :view-box.camel="viewbox"
      >
        <polygon
          id="Path"
          stroke="#0000FF"
          fill="#0000FF"
          points="40 320 40 300 60 300 60 290 70 290 70 280 80 280 80 90 60 90 60 80 40 80 40 30 100 30 100 60 130 60 130 30 180 30 180 60 220 60 220 30 280 30 280 80 260 80 260 90 240 90 240 280 250 280 250 290 260 290 260 300 280 300 280 320"
        ></polygon>
      </svg>
      <!-- <circle
        @click="moveit"
        :cx="xCoord * sideLength - sideLength / 2"
        :cy="yCoord * sideLength - sideLength / 2"
        :r="sideLength * 0.4"
        stroke="black"
        stroke-width="3"
        fill="red"
      /> -->
    </g>
    <!-- <g
      id="Page-1"
      stroke="none"
      stroke-width="1"
      fill="none"
      fill-rule="evenodd"
    >
      <g id="Group" transform="translate(1.000000, 1.000000)">
        <polygon
          id="Path"
          stroke="#0000FF"
          fill="#0000FF"
          points="40 320 40 300 60 300 60 290 70 290 70 280 80 280 80 90 60 90 60 80 40 80 40 30 100 30 100 60 130 60 130 30 180 30 180 60 220 60 220 30 280 30 280 80 260 80 260 90 240 90 240 280 250 280 250 290 260 290 260 300 280 300 280 320"
        ></polygon>
        <polygon
          id="Path-2"
          stroke="#0000FF"
          points="0 320 320 320 320 180.186177 320 0 0 0"
        ></polygon>
        <polygon
          id="Path-3"
          fill="#FFFFFF"
          points="70 310 70 300 80 300 80 290 90 290 90 80 70 80 70 70 50 70 50 40 90 40 90 70 140 70 140 40 170 40 170 70 230 70 230 40 270 40 270 70 250 70 250 80 230 80 230 290 240 290 240 300 250 300 250 310"
        ></polygon>
      </g>
    </g> -->
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
      w: 953.73,
      h: 984,
      vbx: 0,
      vby: 0,
      //vbw: 953.73,
      vbh: 984,
      aCoord: 300,
      bCoord: 300,
      anothersideLength: 100,
    };
  },
  computed: {
    viewbox() {
      const { vbx, vby, vbh } = this.$data;
      return [vbx, vby, this.sideLength, vbh].join(" ");
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