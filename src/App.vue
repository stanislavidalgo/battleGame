<template>
  <div class="flex">
    <div>
      <CharacterBox
        :hp="playerHp"
        :img="
          'https://www.techinn.com/f/13768/137680875_4/bandai-super-saiyan-god-super-saiyan-son-goku-model-kit-dragon-ball-super-15-cm.jpg'
        "
      />
      <div>
        <img
          v-for="(weapon, index) in weapons"
          :key="index"
          @click="setWeapon(index)"
          :src="weapon.img"
          :class="[currentWeapon === index ? 'checked' : '']"
          class="weapon"
        />
      </div>
    </div>

    <div class="flex-col">
      <h3>Gold: {{ gold }}</h3>
      <div class="attackButton" @click="attack">Attack</div>
      <img
        src="https://preview.pixlr.com/images/450nwm/100/1/1001468594.jpg"
        alt="potion"
        @click="buyPotion"
      />
    </div>

    <CharacterBox :hp="enemyHp" :img="enemies[currentEnemy].img" />
  </div>
</template>

<script>
import CharacterBox from "./components/CharacterBox.vue";
export default {
  name: "App",
  components: {
    CharacterBox,
  },
  data() {
    return {
      playerHp: 100,
      enemyHp: 100,
      currentEnemy: 0,
      currentWeapon: 0,
      gold: 100,
      playerDamage: 15,
      enemies: [
        {
          name: "Goblin",
          img: "https://i.imgur.com/yBh7Fn4.png",
          maxDamage: 12,
        },
        {
          name: "Troll",
          img:
            "https://i.pinimg.com/originals/8d/7f/d8/8d7fd8ae9fcd6060497c628e1c7944b4.jpg",
          maxDamage: 8,
        },

        {
          name: "Witch",
          img:
            "https://i.pinimg.com/originals/c0/da/c0/c0dac0da46b4c59534cf898b1967d523.png",
          maxDamage: 15,
        },
      ],
      weapons: [
        {
          img:
            "https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcRjeRzenAFh9nuqc0sexfw63azpjKmulkubHg&usqp=CAU",
          damage: 10,
          effect: "gives player 25% chance to doge enemy attacks",
        },
        {
          img:
            "https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcRYLtdkk7fwbEwdjNpuL0Oo1ka5A7z0PhL34Q&usqp=CAU",
          damage: 12,
          effect: "heals player on every enemy hit from 0 to 5 hit points",
        },
        {
          img: "https://preview.pixlr.com/images/800wm/100/1/1001468630.jpg",
          damage: 7,
          effect: "has a 50% chance to hit enemy two times in a row",
        },
      ],
    };
  },

  methods: {
    randomNum(num) {
      return Math.round(Math.random() * num);
    },

    attack() {
      const playerDamage = this.randomNum(this.playerDamage);
      const enemyDamage = this.randomNum(
        this.enemies[this.currentEnemy].maxDamage
      );

      this.playerHp -= enemyDamage;
      this.enemyHp -= playerDamage;

      this.gold += this.randomNum(10);

      this.weaponSpecials(enemyDamage, playerDamage);
    },

    weaponSpecials(enemyDamage, playerDamage) {
      const chance = (num) => Math.floor(Math.random() * num);

      if (this.currentWeapon === 0) {
        if (chance(4) === 0) this.playerHp += enemyDamage;
      }

      if (this.currentWeapon === 1) {
        this.playerHp += chance(6);
      }

      if (this.currentWeapon === 1) {
        if (chance(2) === 0) {
          this.enemyHp -= playerDamage;
        }
      }
    },
    setWeapon(index) {
      this.playerDamage = this.weapons[index].damage;
      this.currentWeapon = index;
    },

    buyPotion() {
      if (this.gold >= 50) {
        this.playerHp = 100;
        this.gold -= 50;
      }
    },
  },

  watch: {
    enemyHp: function(newHp) {
      if (newHp <= 0) {
        this.enemyHp = 100;
        this.currentEnemy = this.randomNum(2);
      }
    },
    gold: (newGold) => {
      console.log("this is new gold" + newGold);
    },
  },
};
</script>

<style>
body {
  margin: 50px;
  font-family: monospace;
}

.flex {
  display: flex;
  justify-content: space-between;
}

.attackButton {
  padding: 10px;
  text-align: center;
  background-color: rgba(207, 99, 99, 0.582);
  color: white;
  font-size: 1.6em;
  cursor: pointer;
}

.flex-col {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  text-align: center;
}

.flex-col img {
  height: 100px;
  cursor: pointer;
}

.weapon {
  height: 100px;
  cursor: pointer;
}

.checked {
  border: 1px solid rgb(45, 90, 45);
}
</style>
