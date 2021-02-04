<template>
  <div class="hello">
    <b-field label="Name">
      <b-input v-model="chessset.chessset"></b-input>
    </b-field>

    <!-- <button @click="toggleVis" class="button">Tog</button>
    <button @click="incrementSpot" class="button">Down</button>
    <button @click="incrementSpot" class="button">Left</button>
    <button @click="incrementSpot" class="button">Right</button> -->
    <b-field label="pieceX">
      <b-input v-model="pieceX"></b-input>
    </b-field>
    <section>
      <b-field label="SideLength">
        <b-slider :min="1" :max="70" v-model="sideLength"></b-slider>
        <b-slider :min="1" :max="noCols" v-model="pieceY"></b-slider>
      </b-field>
      <b-field label="Simple">
        <b-slider :min="1" :max="noRows" v-model="pieceX"></b-slider>
        <b-slider :min="1" :max="noCols" v-model="pieceY"></b-slider>
      </b-field>
    </section>
    <div class="columns">
      <div class="column"></div>
      <div class="column is-centered">
        <svg :width="noCols * sideLength" :height="noCols * sideLength">
          <g v-for="(item, rowIndex) in noRows" :key="rowIndex">
            <BoardSquare
              v-for="(item, colIndex) in noCols"
              :id="key"
              :xCoord="colIndex"
              :yCoord="rowIndex"
              :key="colIndex * key"
              :sideLength="sideLength"
              :chessset="chessset"
              @emitPixel="emitPixel"
            />
          </g>
          <!-- <g>
            <GamePiece
              ref="child"
              :xCoord="pieceX"
              :yCoord="pieceY"
              :sideLength="sideLength"
            />
          </g> -->
        </svg>
      </div>
      <div
        class="column"
        :xCoord="pieceX"
        :yCoord="pieceY"
        :coordArray="pieceArray"
      ></div>
    </div>
  </div>
</template>

<script>
import BoardSquare from "@/components/BoardSquare.vue";
//import GamePiece from "@/components/GamePiece.vue";
import chessset from "@/assets/chessset.json";
export default {
  name: "HelloWorld",
  components: {
    BoardSquare,
    // GamePiece,
  },
  methods: {
    emitPixel({ xOne, yOne }) {
      this.pieceX = xOne;
      this.pieceY = yOne;
    },
  },

  data() {
    return {
      chessset: chessset,
      noRows: 8,
      noCols: 8,
      sideLength: 50,
      randomthing: [],
      pieceX: 2,
      pieceY: 5,
      pieceArray: [200, 200],
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
