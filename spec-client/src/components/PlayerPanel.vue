<template>
  <div class="player-panel flex" :class="{
    'dead-flash': !data.isAlive && !data.isSpawning,
    'highlight': highlight
  }">
    <div class="avatar-container">
      <img class="dead-icon" src="icons/dead.png" v-if="!data.isAlive && !data.isSpawning">
      <img class="avatar" :class="{ dead: !data.isAlive && !data.isSpawning }" :src="data.avatar">
    </div>
    <div class="info flex">
      <div class="health-bar-container flex">
        <div class="health-bar health-bar-dmg" :style="{ width: data.health + '%' }" v-if="!data.isSpawning"></div>
      </div>
      <div class="health-bar-container flex">
        <div class="health-bar bg-color" :style="{ width: data.health + '%' }" v-if="data.isAlive"></div>
      </div>
      <div class="name">{{data.name}}</div>
      <div class="health align">{{health}}</div>

      <div class="break"></div>
      <div class="stats">
        <div class="icon-container">
          <img class="xp-icon" :src="'icons/' + rank + '.png'">
        </div>
        <div class="xp">{{data.xp}}</div>
        <div class="icon-container">
          <img class="deaths-icon" src="icons/skull.png">
        </div>
        <div class="deaths">{{data.deaths}}</div>
      </div>
      <div class="class">{{className}}</div>
      <div class="spacer"></div>
      <img class="weapon-icon" v-if="data.isAlive && data.activeWeapon" :src="'icons/' + data.activeWeapon + '.png'">
    </div>
</div>
</template>

<script>
export default {
  name: 'PlayerPanel',
  props: {
    data: Object,
    highlight: Boolean,
  },
  computed: {
    health() {
      if (this.data.isAlive) {
        return this.data.health;
      } else if (this.data.isSpawning) {
        return "";
      } else {
        return "Dead";
      }
    },
    className() {
      if (this.data.isAlive) {
        return this.data.class;
      } else {
        return "";
      }
    },
    rank() {
      if (this.data.xp < 0) {
        return "ranklessdog"
      } else if (this.data.xp < 4) {
        return "private"
      } else if (this.data.xp < 10) {
        return "corporal"
      } else if (this.data.xp < 20) {
        return "sergeant"
      } else {
        return "lieutenant"
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.player-panel {
  font-weight: bold;
  display: flex;
  overflow: hidden;
  margin: 2px 0;
  padding: 2px;
  width: 420px;
  color: white;
  text-shadow: 1px 1px 2px #222;
  background-color: rgba(0,0,0,0.5);
  box-sizing: border-box;
}

.player-panel.highlight {
  border: 2px solid rgba(255,255,255,0.8);
  padding: 0;
}

.break {
  flex: 0 0 100%;
  height: 0;
}

.spacer {
  flex: 1 1 auto;
}

.avatar-container {
  flex: 0 0 auto;
  height: 64px;
  width: 66px;
}

.avatar {
  width: 64px;
  height: 64px;
}

.dead {
  /* filter: sepia(100%) hue-rotate(310deg) saturate(600%) brightness(50%); */
  filter: grayscale();
}

.dead-icon {
  position: absolute;
  height: 64px;
  z-index: 100;
  background-color: rgba(255,0,0,0.3);
}

.info {
  flex: 1 1 auto;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  box-sizing: border-box;
  padding: 4px 12px;
  position: relative;
  z-index: 10;
}

.name {
  font-size: 16px;
  flex: 1 0 160px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.class {
  width: 80px;
  font-size: 14px;
  color: #eee;
  align-self: flex-end;
  padding-bottom: 3px;
  text-align: center;
}

.health {
  width: 60px;
  color: #eee;
  text-align: center;
}

.health-bar-container {
  display: flex;
  position: absolute;
  z-index: -1;
  height: 100%;
  width: 100%;
  left: 0px;
  box-sizing: border-box;
  filter: brightness(75%);
}

.health-bar {
  height: 50%;
  left: 0px;
}

.health-bar-dmg {
  background: none;
  background-color: rgba(255,0,0,0.5);
  transition: width 1s ease 0.2s;
}

.stats {
  display: flex;
  font-size: 16px;
  line-height: 22px;
  align-self: flex-end;
}

.icon-container {
  width: 18px;
  height: 18px;
  margin: 0 6px;
  align-self: center;
  text-align: right;
}

.xp-icon, .deaths-icon {
  height: 18px;
}

.xp, .deaths {
  width: 36px;
  text-align: left;
}

.weapon {
  font-size: 12px;
  color: #ccc;
}

.weapon-icon {
  max-height: 22px;
  max-width: 100px;
  position: relative;
  top: 2px;
}

@keyframes onPlayerDead {
  0% {
    background-color: rgba(255,0,0,0.5);
  }
  100% {
    background-color: rgba(0,0,0,0.5);
  }
}

.dead-flash {
  animation: onPlayerDead 2s ease;
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
