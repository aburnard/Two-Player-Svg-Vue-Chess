<template>
  <div class="hello">
    <b-field label="CurrentAscii">
      <b-input v-model="currentAscii"> </b-input>
    </b-field>
    <b-field label="Name">
      <b-input v-model="chessset.board[selectedX][selectedY]"> </b-input>
    </b-field>
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
              :selectedSquare="{ pieceX, pieceY }"
              :chessJsBoard="game.board()"
            />
          </g>
        </svg>
        <b-field>
          <b-switch v-model="aSquareSelected">
            {{ aSquareSelected }}
          </b-switch>
        </b-field>
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
import Chess from "chess.js";
import BoardSquare from "@/components/BoardSquare.vue";
//import GamePiece from "@/components/GamePiece.vue";
import chessset from "@/assets/chessset.json";
export default {
  name: "HelloWorld",
  created() {
    this.game = new Chess();
  },
  components: {
    BoardSquare,
    // GamePiece,
  },
  methods: {
    emitPixel({ xOne, yOne }) {
      const numberToLetter = {
        1: "a",
        2: "b",
        3: "c",
        4: "d",
        5: "e",
        6: "f",
        7: "g",
        8: "h",
      };
      const reverseYCoord = {
        1: 8,
        2: 7,
        3: 6,
        4: 5,
        5: 4,
        6: 3,
        7: 2,
        8: 1,
      };
      //chess.move({ from: 'g2', to: 'g3' })
      // this.pieceX = xOne;
      // this.pieceY = yOne;
      // this.game.move("e4");
      if (this.aSquareSelected) {
        this.arrayPlaceHolder = chessset.board;
        this.arrayPlaceHolder[yOne - 1][xOne - 1] = this.currentSelectedPiece;
        chessset.board = this.arrayPlaceHolder.slice(0);
        this.arrayPlaceHolder[this.pieceY - 1][this.pieceX - 1] = 0;
        chessset.board = this.arrayPlaceHolder.slice(0);
        this.moveTo = "" + numberToLetter[xOne] + reverseYCoord[yOne];

        this.game.move({ from: this.moveFrom, to: this.moveTo });
        this.aSquareSelected = false;
      } else {
        this.pieceX = xOne;
        this.pieceY = yOne;
        this.currentSelectedPiece = chessset.board[yOne - 1][xOne - 1];
        this.moveFrom = "" + numberToLetter[xOne] + reverseYCoord[yOne];

        this.aSquareSelected = true;
      }
    },
  },

  data() {
    return {
      moveFrom: "",
      moveTo: "",
      chessset: chessset,
      noRows: 8,
      noCols: 8,
      sideLength: 50,
      randomthing: [],
      selectedX: 0,
      selectedY: 0,
      pieceX: 2,
      pieceY: 5,
      pieceArray: [200, 200],
      arrayPlaceHolder: [],
      currentSelectedPiece: 0,
      aSquareSelected: false,
    };
  },
  computed: {
    // a computed getter
    chessJsBoard: function () {
      return this.game.board();
    },
    currentAscii: function () {
      // `this` points to the vm instance
      return this.game.ascii();
    },
  },
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body {
  background-color: rgb(20, 5, 65);
}
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
