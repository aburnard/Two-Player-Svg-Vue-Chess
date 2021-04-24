<template>
  <div :style="{ backgroundColor: backgroundByTurn }" class="hello">
    <section>
      <!-- <b-field label="SideLength">
        <b-slider :min="1" :max="70" v-model="sideLength"></b-slider>
      </b-field> -->
    </section>
    <div class="columns">
      <div class="column"></div>
      <div class="column is-centered">
        <svg :width="noCols * sideLength" :height="noCols * sideLength">
          <g v-for="(item, rowIndex) in noRows" :key="rowIndex">
            <BoardSquare
              v-for="(item, colIndex) in noCols"
              :stroke="white"
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

        //this.game.move({ from: this.moveFrom, to: this.moveTo });
        this.aSquareSelected = false;

        this.game.move({
          from: this.moveFrom,
          to: this.moveTo,
          promotion: "q",
        });

        if (this.game.turn() == "w") {
          this.whitesTurn = true;
        } else this.whitesTurn = false;
        if (this.game.in_check() && !this.game.in_checkmate()) {
          this.$buefy.toast.open("Check!");
        }
        if (this.game.in_checkmate()) {
          this.$buefy.toast.open("Checkmate!");
        }
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
      myStyle: {
        backgroundColor: "#16a085",
      },
      whitesTurn: true,
      wbToColor: { w: "white", b: "#47473b" },
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
    currentTurn: function () {
      // `this` points to the vm instance
      return this.game.turn();
    },
    backgroundByTurn() {
      if (this.whitesTurn) {
        return "white";
      } else {
        return "black";
      }
    },
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
