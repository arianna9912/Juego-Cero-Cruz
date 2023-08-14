<template>
  <div class="container">
    <h1 class="center-align">Cero o Cruz?</h1>
    <div class="row">
      <div class="col s12 m6 offset-m3">
        <div class="card" :class="cardColor">
          <div class="card-content">
            <div class="row">
              <div class="col s4 margin-bottom-5" v-for="(cell, index) in squares" :key="index">
                <button class="waves-effect waves-light btn-large square indigo margin-right" :disabled="cell || winner" @click="makeMove(index)">
                  <span class="white-text" :class="{'indigo-text': cell === 'X' || cell === 'O'}">{{ cell }}</span>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-if="winner" class="row">
      <div class="col s12 m6 offset-m3">
        <div class="card">
          <div class="card-content">
            <h4 class="center-align">{{ winner === 'Empate' ? 'Empatados' : 'Felicidades jugador: ' + winner }}</h4>
            <div class="center-align">
              <button class="waves-effect waves-light btn indigo" @click="resetGame">Jugar de nuevo</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      squares: Array(9).fill(null),
      xIsNext: true,
      winner: null,
      cardColor: "indigo lighten-4" // Nueva propiedad de datos para el color de la tarjeta
    };
  },
  methods: {
    makeMove(index) {
      if (this.squares[index] || this.winner) {
        return;
      }
      this.squares.splice(index, 1, this.xIsNext ? "X" : "O");
      this.xIsNext = !this.xIsNext;
      this.calculateWinner();
      if (!this.winner) {
        this.cardColor = "indigo lighten-3"; // Cambiar el color de la tarjeta si no hay ganador
      }
    },
    calculateWinner() {
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
      ];
      for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (
          this.squares[a] &&
          this.squares[a] === this.squares[b] &&
          this.squares[a] === this.squares[c]
        ) {
          this.winner = this.squares[a];
          return;
        }
      }
      if (this.squares.every(cell => cell !== null)) {
        this.winner = "Empate";
      }
    },
    resetGame() {
      this.squares = Array(9).fill(null);
      this.xIsNext = true;
      this.winner = null;
      this.cardColor = "indigo lighten-4"; // Restaurar el color de la tarjeta al valor inicial
    }
  }
};
</script>

<style scoped>
.square {
  width: 100%;
  height: 100px;
  font-size: 48px;
}

.indigo {
  background-color: indigo;
}

.indigo-text {
  color: indigo;
}

.white-text {
  color: white;
}

.center-align {
  text-align: center;
}

.margin-bottom-5 {
  margin-bottom: 5px;
}

.margin-right {
  margin-right: 5px;
}

.indigo.lighten-3 {
  background-color: #5c6bc0;
}
</style>