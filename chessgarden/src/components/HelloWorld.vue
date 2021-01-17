<template>
  <div class="hello">
    <b-collapse :open="false" aria-id="contentIdForA11y1">
      <button
        class="button is-primary"
        slot="trigger"
        aria-controls="contentIdForA11y1"
      >
        Click for Load/Save
      </button>

      <div class="notification">
        <div class="content">
          <h3>FileSaving</h3>
          <section>
            <b-field
              label="Copy and Paste Textarea"
              :label-position="labelPosition"
            >
              <b-input v-model="pixMatrixString" type="textarea"></b-input>
            </b-field>
          </section>

          <section>
            <b-button @click="dumpButton">Dump To Textarea</b-button>
            <b-button @click="loadButton">Load from Textarea</b-button>
            <!-- <b-button @click="newCanvas">newCanvas</b-button> -->
          </section>

          <section>
            <b-button @click="dumpIt()">Download as SVG</b-button>
          </section>
        </div>
      </div>
    </b-collapse>
    <!-- <button @click="toggleVis" class="button">Tog</button>
    <button @click="incrementSpot" class="button">Down</button>
    <button @click="incrementSpot" class="button">Left</button>
    <button @click="incrementSpot" class="button">Right</button> -->
    <b-field>
      <b-switch v-model="blueOrWhite" true-value="blue" false-value="white">
        {{ blueOrWhite }}
      </b-switch>
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
              @emitPixel="emitPixel"
              :pixMatrix="pixMatrix"
              :blueOrWhite="blueOrWhite"
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
import pixMatrix from "@/assets/pixMatrix.json";
import BoardSquare from "@/components/BoardSquare.vue";
//import GamePiece from "@/components/GamePiece.vue";
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
    dumpButton() {
      this.dataPixMat = this.pixMatrix.slice(0);

      this.pixMatrixString = JSON.stringify(this.dataPixMat);
    },
    loadButton() {
      this.dataPixMat = JSON.parse(this.pixMatrixString);
      this.pixMatrix = this.dataPixMat.slice(0);
    },
  },

  data() {
    return {
      pixMatrixString: "",
      isOpen: false,
      pixMatrix: pixMatrix.pixMatrix,
      noRows: 32,
      noCols: 32,
      sideLength: 10,
      randomthing: [],
      dataPixMat: [],
      pieceX: 2,
      pieceY: 5,
      pieceArray: [200, 200],
      blueOrWhite: "blue",
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
