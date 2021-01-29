<template>
  <div class="hello">
    <img
      src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg'%3E%3Cg id='Page-1' stroke='none' stroke-width='1' fill='none' fill-rule='evenodd'%3E%3Cg id='Group'%3E%3Cpolygon id='Path' fill='%231908F9' points='16 128 16 124 20 124 20 120 24 120 24 116 28 116 28 112 32 112 32 108 36 108 36 104 48 104 48 72 32 72 32 68 36 68 36 64 40 64 40 60 44 60 44 56 40 56 40 52 36 52 36 48 32 48 32 44 28 44 28 28 32 28 32 24 36 24 36 20 40 19.6 40 16.4 44 16 44 12 84 12 84 16 88 16 88 20 92 20 92 24 96 24 96 28 100 28 100 44 96 44 96 48 92 48 92 52 88 52 88 56 84 56 84 60 88 60 88 64 92 64 92 68 96 68 96 72 80 72 80 104 92 104 92 108 96 108 96 112 100 112 100 116 104 116 104 120 108 120 108 124 112 124 112 128'%3E%3C/polygon%3E%3Cpolygon id='Path-2' points='128 128 0 128 0 0 128 0'%3E%3C/polygon%3E%3Cpolygon id='Path-3' stroke='%23F9F5F5' fill='%23FBF6F6' points='48 16.4 80 16.4 80 19.6 84 19.6 84 24 88 24 88 28 92 28 92 32 96 32 96 40 92 40 92 44 88 44 88 48 84 48 84 52 80 52 80 64 84 64 84 68 76 68 76 108 88 108 88 112 92 112 92 116 96 116 96 120 100 120 100 124 28 124 28 120 32 120 32 116 36 116 36 112 40 112 40 108 52 108 52 68 44 68 44 64 48 64 48 52 44 52 44 48 40 48 40 44 36 44 36 28 40 28 40 24 48 24'%3E%3C/polygon%3E%3C/g%3E%3C/g%3E%3C/svg%3E"
    />

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
        <svg
          :id="composedSvg"
          :width="noCols * sideLength"
          :height="noCols * sideLength"
        >
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
          <g>
            <GamePiece
              ref="child"
              :xCoord="pieceX"
              :yCoord="pieceY"
              :sideLength="sideLength"
            />
          </g>

          <!-- <g>
            <polygon
              id="Path"
              stroke="#0000FF"
              fill="#0000FF"
              :points="somePoints"
            ></polygon>
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
import GamePiece from "@/components/GamePiece.vue";
export default {
  name: "HelloWorld",
  components: {
    BoardSquare,
    GamePiece,
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
      somePoints:
        "16 128 16 124 20 124 20 120 24 120 24 116 28 116 28 112 32 112 32 108 36 108 36 104 48 104 48 72 32 72 32 68 36 68 36 64 40 64 40 60 44 60 44 56 40 56 40 52 36 52 36 48 32 48 32 44 28 44 28 28 32 28 32 24 36 24 36 20 40 19.6 40 16.4 44 16 44 12 84 12 84 16 88 16 88 20 92 20 92 24 96 24 96 28 100 28 100 44 96 44 96 48 92 48 92 52 88 52 88 56 84 56 84 60 88 60 88 64 92 64 92 68 96 68 96 72 80 72 80 104 92 104 92 108 96 108 96 112 100 112 100 116 104 116 104 120 108 120 108 124 112 124 112 128",
      pixMatrixString: "",
      isOpen: false,
      pixMatrix: pixMatrix.pixMatrix,
      noRows: 8,
      noCols: 8,
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
