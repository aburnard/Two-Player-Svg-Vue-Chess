<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <svg width="20000" height="20000">
      <circle
        cx="400"
        cy="200"
        r="100"
        stroke="green"
        stroke-width="4"
        fill="yellow"
      />
      <g v-for="(item, index) in noSquares" :key="index">
        <BoardSquare :id="key" :xCoord="index" :key="index * key" />
      </g>
    </svg>
  </div>
</template>

<script>
import BoardSquare from "@/components/BoardSquare.vue";
export default {
  name: "HelloWorld",
  components: {
    BoardSquare,
  },
  data() {
    return {
      noSquares: 64,
    };
  },
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
