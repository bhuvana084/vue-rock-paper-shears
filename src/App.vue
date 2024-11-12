<script setup>
import { reactive, ref } from 'vue'

const userChoice = ref(0)
const computerChoice = ref(0)
const winner = ref('')
const choiceMap = ['Rock', 'Paper', 'Scissors']
const userPlayed = ref('')
const gameHasBegun = ref(false)
const score = reactive({
  user: 0,
  computer: 0,
})

const playGame = () => {
  gameHasBegun.value = true
  userPlayed.value = choiceMap[userChoice.value - 1]
  computerChoice.value = getRndInteger(1, 3)
  if (userChoice.value == computerChoice.value) {
    winner.value = 'none'
  } else if (
    (userChoice.value == 1 && computerChoice.value == 3) ||
    (userChoice.value == 2 && computerChoice.value == 1) ||
    (userChoice.value == 3 && computerChoice.value == 2)
  ) {
    winner.value = 'user'
    score.user++
  } else {
    winner.value = 'computer'
    score.computer++
  }
}

const getRndInteger = (min, max) => {
  return Math.floor(Math.random() * (max - min + 1)) + min
}

const resetGame = () => {
  gameHasBegun.value = false
  score.user = 0
  score.computer = 0
  userPlayed.value = ''
  userChoice.value = 0
  computerChoice.value = 0
  winner.value = ''
}
</script>

<template>
  <main>
    <div style="text-align: center">
      <img
        alt="Vue logo"
        class="logo"
        src="./assets/logo.svg"
        width="75"
        height="75"
        style="text-align: center"
      />
    </div>
    <form @submit.prevent="playGame">
      <div style="text-align: center">
        <h1>Let's Play Rock, Paper, Scissors</h1>
        <label for="user_input">What do you choose?</label>
        <div>
          <label for="rock">
            <input type="radio" id="rock" value="1" v-model="userChoice" />
            <img src="./assets/rock.png" style="height: 100px" alt="Rock" />
          </label>
          <label for="paper">
            <input type="radio" id="paper" value="2" v-model="userChoice" />
            <img src="./assets/paper.png" style="height: 100px" alt="Paper" />
          </label>
          <label for="shears">
            <input type="radio" id="shears" value="3" v-model="userChoice" />
            <img src="./assets/scissors.png" style="height: 100px" alt="Scissors" />
          </label>
        </div>
        <br />
        <input type="submit" value="Play" />
        <button type="button" @click="resetGame">Reset</button>
      </div>
    </form>
    <div v-if="gameHasBegun" style="text-align: center; padding-top: 20px">
      <div v-if="winner == 'user'"><h1>You WIN!!</h1></div>
      <div v-else-if="winner == 'computer'"><h1>You LOSE!!</h1></div>
      <div v-else><h1>It's a DRAW!!</h1></div>

      <div class="parent">
        <div class="child">
          <p class="dark">User</p>
          <p>
            Choice: <span class="dark">{{ userPlayed }}</span>
          </p>
          <p>
            Score : <span class="dark">{{ score.user }}</span>
          </p>
        </div>
        <div class="child">
          <p class="dark">Computer</p>
          <p>
            Choice: <span class="dark">{{ choiceMap[computerChoice - 1] }}</span>
          </p>
          <p>
            Score : <span class="dark">{{ score.computer }}</span>
          </p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
input[type='radio'] {
  /* visibility: hidden; */
}
input[type='radio']:checked {
  /* visibility: hidden; */
  border: 2 px solid #35495e;
  border-radius: 5px;
}
button,
input[type='submit'] {
  color: white;
  font-weight: bold;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  margin: 10px;
  font-size: 15px;
  cursor: pointer;
  background-color: #35495e;
}
button:hover,
input[type='submit']:hover {
  background-color: #42b883;
}

.dark {
  color: #35495e;
  font-weight: bold;
}

.parent {
  margin: 10px;
  padding: 20px;
  text-align: center;
  color: white;
}
.child {
  display: inline-block;
  padding: 10px;
  margin: 10px;
  vertical-align: middle;
  background-color: #42b883;
  border-radius: 5px;
  box-shadow: 0 8px 16px 0 #35495e;
  font-size: 15px;
}
</style>
