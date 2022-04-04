<template>
  <div class="container w-50">
    <!-- First Screen to choose the word lenght  -->
    <div v-if="screen === 'startGame'" id="startGame">
      <div class="row mb-5">
        <label for="wordLenght" class="col-lg-4 font-weight-bold"
          >Word Lenght:</label
        >
        <div class="col-lg-8">
          <select
            name="wordLenght"
            id="wordLenght"
            class="form-select"
            v-model="wordLenght"
          >
            >
            <option v-for="option in options" :key="option" :value="option">
              {{ option }}
            </option>
          </select>
        </div>
      </div>
      <div class="mb-3">
        <div class="col-lg-8 text-left">
          <p>1. Choose Word Lenght</p>
          <p>2. Press <span class="font-weight-bold">Play!</span></p>
          <p>3. How many anagrams you can find in a minute?</p>
        </div>
      </div>
      <div class="row">
        <button class="btn btn-primary col" @click="play">Play</button>
      </div>
    </div>
    <!-- End of first screen -->
    <!-- Play Game screen -->
    <div v-else-if="screen === 'play'" id="game">
      <div class="row">
        <div class="col font-weight-bolder text-left">
          <Score :value="score" />
        </div>
        <div class="col font-weight-bolder text-right">
          <Timer :timeLeft="timeLeft" />
        </div>
      </div>
      <hr />
      <div class="row mb-4">
        <div class="col w-30">
          <Anagram :word="randomWord" :remain="randomAnagramLenght" />
        </div>
      </div>
      <div class="row mb-5">
        <div class="col w-30">
          <input
            type="text"
            placeholder="Type here"
            v-model="input"
            @keyup.enter="checkAnswer"
          />
        </div>
      </div>
      <div class="row">
        <ol>
          <li v-for="a in answers" :key="a">{{ a }}</li>
        </ol>
      </div>
    </div>
    <!-- End of play game screen -->
    <!-- Game result screen -->
    <div v-if="screen === 'GameResult'">
      <GameResult
        :score="score"
        @play-button-click="play"
        @start-again-click="start"
      />
    </div>
  </div>
</template>
<script>
import Score from "./Score.vue";
import Timer from "./Timer.vue";
import Anagram from "./Anagram.vue";
import GameResult from "./GameResult.vue";
import Words from "../words.json";

export default {
  name: "Main",

  components: {
    Score,
    Timer,
    Anagram,
    GameResult,
  },

  data: function () {
    return {
      screen: "startGame",
      wordLenght: "5",
      options: [5, 6, 7, 8],
      input: "",
      words: Words,
      selectedAnagram: [], //outer array
      randomAnagram: [], //inner array
      randomAnagramLenght: 0,
      randomWord: "",
      score: 0,
      answers: [],
      timeLeft: 0,
      gameLenght: 60,
    };
  },
  methods: {
    // display the screen to choose the word's lenght
    start() {
      this.screen = "startGame";
    },
    // display the screen to play game
    play() {
      this.screen = "play";
      this.findAnagram();
      this.startTimer();
      this.reset();
    },
    // display the game result screen with user's score
    gameResult() {
      this.screen = "GameResult";
    },
    //
    reset() {
      this.score = 0;
      this.answers = [];
      this.clearInput();
    },
    // find all arrays with user selected word lenght
    findAnagram() {
      this.selectedAnagram = this.words[this.wordLenght];
      this.findRandomAnagram();
    },
    // generate a random integer between 0 and array's lenght
    randomInt(length) {
      return Math.floor(Math.random() * (length - 1) + 1);
    },
    // find the final array to play, randomly choosed by array lenght
    findRandomAnagram() {
      this.randomAnagram =
        this.selectedAnagram[this.randomInt(this.selectedAnagram.length)];
      this.randomAnagramLenght = this.randomAnagram.length;
      this.findRandomWord(this.randomAnagram.length);
    },
    // find a random word to display for user from selected anagram
    findRandomWord(length) {
      this.randomWord = this.randomAnagram[this.randomInt(length)];
    },
    //check user input for correct answers
    checkAnswer() {
      for (let a of this.randomAnagram) {
        if (a === this.input && a !== this.randomWord) {
          this.score++;
          this.answers.push(this.input); //save user inputs in an array
          this.randomAnagramLenght--;
        }
        // if user is done with one anagram display the next one
        if (this.randomAnagramLenght == 1) {
          this.answers = [];
          this.findRandomAnagram();
        }
      }
      this.clearInput();
    },
    // clear text inside input text box
    clearInput() {
      this.input = "";
    },
    startTimer() {
      this.timeLeft = this.gameLenght;
      if (this.timeLeft > 0) {
        this.timer = setInterval(() => {
          this.timeLeft--;
          if (this.timeLeft === 0) {
            clearInterval(this.timer);
            this.gameResult();
          }
        }, 1000);
      }
    },
  },
};
</script>