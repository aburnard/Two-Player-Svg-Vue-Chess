<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <svg width="20000" height="20000">
      <g v-for="(item, rowIndex) in noRows" :key="rowIndex">
        <BoardSquare
          v-for="(item, colIndex) in noCols"
          :id="key"
          :xCoord="colIndex"
          :yCoord="rowIndex"
          :key="colIndex * key"
        />
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
      noRows: 8,
      noCols: 8,
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
