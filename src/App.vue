<template>
  <div>
    <div class="ui segment">
      <div class="ui two column very relaxed grid">
        <div class="column">
          <p>you</p>
          <p class="bar" :style="{width: playerHealth*3 +'px'}">{{ playerHealth }}</p>
        </div>
        <div class="column">
          <p>monster</p>
          <p class="bar" :style="{width: monsterHealth*3 +'px'}">{{ monsterHealth }}</p>
        </div>
      </div>
      <div class="ui vertical divider">vs</div>
    </div>
    <div class="container" v-if="!gameIsRunning">
      <button class="ui button" @click="startGame">START NEW GAME</button>
    </div>
    <div class="container" v-else>
      <button class="ui orange basic button" @click="attack">ATTACK</button>
      <button class="ui yellow basic button" @click="specialAttack">SPECIAL ATTACK</button>
      <button class="ui olive basic button" @click="heal">HEAL</button>
      <button class="ui violet basic button" @click="giveUp">GIVE UP</button>
    </div>
    <div class="text" v-if="turns.length > 0">
      <ul>
        <li
          v-for="(turn, index) in turns"
          :key="index"
          :class="{'player-turn': turn.isPlayer, 'monster-turn': !turn.isPlayer}"
        >{{ turn.text}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      gameIsRunning: false,
      playerHealth: 100,
      monsterHealth: 100,
      turns: []
    };
  },
  methods: {
    startGame() {
      this.gameIsRunning = true;
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.turns = [];
    },
    attack() {
      let damage = this.calculateDamage(3, 10);
      this.monsterHealth -= damage;
      this.turns.unshift({
        isPlayer: true,
        text: "Plyer hits monster hard for" + damage
      });
      if (this.checkWin()) {
        return;
      }
      this.monsterAttacks();
    },
    specialAttack() {
      let damage = this.calculateDamage(10, 20);
      this.monsterHealth -= damage;
      if (this.checkWin()) {
        return;
      }
      this.monsterAttacks();
    },
    heal() {
      if (this.playerHealth <= 90) {
        this.playerHealth += 10;
      } else {
        this.playerHealth = 100;
      }
      this.turns.unshift({
        isPlayer: true,
        text: "Plyer hits heals for 10"
      });
      this.monsterAttacks;
    },
    giveUp() {
      this.gameIsRunning = false;
    },
    calculateDamage(min, max) {
      return Math.max(Math.floor(Math.random() * max) + 1, min);
    },
    checkWin() {
      if (this.monsterHealth <= 0) {
        confirm("youwon!New game?")
          ? this.startGame()
          : (this.gameIsRunning = false);
        return true;
      } else if (this.playerHealth <= 0) {
        confirm("you lost!New game?")
          ? this.startGame()
          : (this.gameIsRunning = false);
        return true;
      }
      return false;
    },
    monsterAttacks() {
      let damage = this.calculateDamage(5, 12);
      this.playerHealth -= damage;
      this.checkWin();
      this.turns.unshift({
        isPlayer: false,
        text: "Monster hits monster for" + damage
      });
    }
  }
};
</script>

<style>
.bar {
  display: inline-block;
  background-color: pink;
  text-align: center;
  width: 100px;
  height: 30px;
}
.container {
  text-align: center;
  margin: 20px;
}
.ui.button {
  margin-left: 20px;
}
.text {
  text-align: center;
  margin: 20px;
  border: thick double #32a1ce;
}
ul {
  list-style: none;
}
.player-turn {
  color: aqua;
}
.monster-turn {
  color: red;
}
</style>
