<template>
  <div class="app">
    <div class="content">
      <div v-if="gameOver"  data-aos="fade-down" class="descr">
        <h4>{{ descr }}</h4>
      </div>

      <div v-if="gameOver"  data-aos="fade-up" class="start">
        <button @click="startGame">Play</button>
      </div>
      <div class="game" v-if="!gameOver">
        <h4  data-aos="fade-down">Tic Tac By Soufiane <span style="color: red">&#10084;</span></h4>
        <div  data-aos="zoom-in" data-aos-duration="1100" id="board">
          <div
            class="square black"
            :class="square[n] === 'X' ? 'blue' : 'red'"
            v-for="n in 9"
            :key="n"
            @click="clickedSquare(n)"
          >
            {{ square[n] }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  name: "App",
  setup() {
    let descr = ref("Start Game Now!");
    let currentTurn = "X";
    let square = ref([]);
    let gameOver = ref(true);
    for (let i = 0; i <= 9; i++) square.value.push(" ");
    square.value[0] = "nothing";
    function clickedSquare(n) {
      if (square.value[n] !== " ") return;
      square.value[n] = currentTurn;
      setTimeout(isWinner, 1000);
      setTimeout(isCat, 1000);
      console.log(currentTurn);
      currentTurn === "X" ? (currentTurn = "O") : (currentTurn = "X");
    }
    function isWinner() {
      const lines = [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9],
        [1, 4, 7],
        [2, 5, 8],
        [3, 6, 9],
        [1, 5, 9],
        [3, 5, 7],
      ];
      for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (
          square.value[a] !== " " &&
          square.value[a] === square.value[b] &&
          square.value[a] === square.value[c]
        ) {
          gameOver.value = true;
            descr.value = `'${square.value[a]}' Winner Congratulation!`;
        }
      }
      
    }
    function isCat() {
      let count = 0;
      for (let i = 0; i <= 9; i++) {
        if (square.value[i] !== " ") {
          count++;
        }
      }
      if (count === 10) {
        descr.value = "Game Over!";
        isWinner();
        gameOver.value = true;
      }
    }
    function startGame() {
      square.value.splice(0, square.value.length);
      gameOver.value = false;
      for (let i = 0; i <= 9; i++) square.value.push(" ");
      square.value[0] = "nothing";
      currentTurn = 'X';
    }
    return {
      square,
      clickedSquare,
      gameOver,
      startGame,
      descr,
    };
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  color: #171717;
  font-family: "Poppins", sans-serif;
}
.descr {
  margin: 30px auto;
  font-weight: 600;
  font-size: 1.5rem;
  letter-spacing: 1px;
}
.start {
  margin: 0 auto;
}
.black {
  color: #171717;
}
.start button {
  background: green;
  color: #fff;
  font-weight: 600;
  padding: 10px 20px;
  border-radius: 0.3rem;
  cursor: pointer;
  border: none;
  letter-spacing: 2px;
  font-size:1.7rem;
}
.blue {
  color: #0984e3;
}
.red {
  color: #d63031;
}
.content {
  padding: 40px;
  width: 45%;
  height: auto;
  overflow: hidden;
  text-align: center;
  border-radius: 1rem;
  background-color: #fff;
}
.square {
  width: calc(400px / 3);
  cursor: pointer;
  height: calc(400px / 3);
  background-color: #fff;
  border: 1px solid #ccc;
  display: flex;
  font-weight: bold;
  font-size: 1.7rem;
  justify-content: center;
  align-items: center;
}
#board {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  height: 400px;
  width: 400px;
  margin: 20px auto;
}
.app {
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(
      rgba(0, 0, 0, 0.9),
      rgba(0, 0, 0, 0.8),
      rgba(0, 0, 0, 0.7),
      rgba(0, 0, 0, 0.6)
    ),
    url("./assets/bg.jpg");
}
@media screen and (max-width: 1100px){
  .content{
    width:60%;
  }
}
@media screen and (max-width: 767px){
  .content{
    width:80%;
  }
}
@media screen and (max-width: 600px){
  .content{
    width:90%;
  }
}
@media screen and (max-width: 500px){
  .content{
    width:90%;
  }
  .square {
  width: calc(250px / 3);
  height: calc(250px / 3);
  font-size: 1rem;
}
#board {
  height: 250px;
  width:  250px;
}
.descr{
  font-size:1rem;
}
.start button{
  font-size:1rem;
}
}
@media screen and (max-width: 350px){
  .content{
    width:90%;
  }
  .square {
  width: calc(150px / 3);
  height: calc(150px / 3);
}
#board {
  height: 150px;
  width:  150px;
}
}
</style>
