<template>
  <div id="app">
      <div class="content">
          <audio id="dionSound" loop>
              <source src="./assets/dion.mp3">
          </audio>
          <audio id="sys_enchant_success">
              <source src="./assets/sys_enchant_success.mp3">
          </audio>
          <audio id="sys_enchant_failed">
              <source src="./assets/sys_enchant_failed.mp3">
          </audio>
      </div>
<!--    <img alt="Vue logo" src="./assets/logo.png">-->
    <div class="window">
      <div class="window__workSpace">
          <img  v-if="currentCount != 0" class="window__workSpace_weapon" :class="{
            weapon_4: currentCount == 4,
            weapon_5: currentCount == 5,
            weapon_6: currentCount == 6,
            weapon_7: currentCount == 7,
            weapon_8: currentCount == 8,
            weapon_9: currentCount == 9,
            weapon_10: currentCount == 10,
            weapon_11: currentCount == 11,
            weapon_12: currentCount == 12,
            weapon_13: currentCount == 13,
            weapon_14: currentCount == 14,
            weapon_15: currentCount == 15,
            weapon_16: currentCount >= 16,
          }" src="./assets/weapon.png">
          <img v-if="currentCount == 0 && start != true" class="window__workSpace_weapon" src="./assets/crystal.png">
        <div  v-if="currentCount != 0" class="window__workSpace_count">
            +{{currentCount}} <span class="btn__trade" v-if="currentCount> 3" @click="sellWeapon"> {{currentCount*tax_weapon}}кк</span>
        </div>
          <div class="window__workSpace_info">
              <span>Gold: {{gold}}kk </span><br>
              <span>Crystal: {{crystal}}</span> <span class="btn__trade" v-if="crystal > 0" @click="sellCrystal"> {{crystal*tax_crystal}}кк</span><br>
              <span>MAX: {{max}}</span>
          </div>

      </div>
      <div class="window__footer">
        <a>
            <img  class="window__footer__btn1" src="./assets/button1.png" @click="calcEnhance">
        </a>
        <a>
          <img  class="window__footer__btn2" src="./assets/close.png" @click="reload">
        </a>
      </div>
        <div class="footer">
            <img class="donate" src="./assets/donate.png" @click="donate"/>
        </div>
    </div>

  </div>
</template>

<script>

export default {
  name: 'app',
  data() {
    return {
        start: true,
        currentCount: 0,
        wait: false,
        gold: 1000,
        crystal: 0,
        max: 0,
        tax_weapon: 50,
        tax_crystal: 0.8,
        donate_count: 0,
        total_click: 0
    }
  },
  methods: {
      calcEnhance: function () {
          if (this.start == true) this.start = false
          if (this.gold < 0) {
              alert("PIZDEC")
              return 0;
          }
          this.wait = true;

          let ranV = Math.floor(Math.random() * Math.floor(100));
          // eslint-disable-next-line no-console
          console.log(ranV);
          if (ranV > 66 && this.currentCount > 2) {
              let audioFailed = document.getElementById("sys_enchant_failed")
              audioFailed.play();
              if (this.max < this.currentCount) this.max = this.currentCount
              this.crystal += this.currentCount * 25
              this.currentCount = 0;
          } else {
              if (this.currentCount == 0) {
                  this.gold -= 125
              }
              let audioSuccess = document.getElementById("sys_enchant_success")

              audioSuccess.playbackRate = 1.8
              audioSuccess.play();
              this.gold -= 20;
              this.currentCount++;
          }
          if (this.gold < 0) {
              alert("PIZDEC")
              return 0;
          }
          this.total_click++
      },
      sellWeapon() {
        this.gold += this.currentCount * this.tax_weapon;
        this.currentCount = 0
      },
      sellCrystal(){
        this.gold += this.crystal*this.tax_crystal;
        this.crystal = 0
      },
      donate(){
          if(this.donate_count == 0) {
              alert("ДАНАТИМ")
              this.gold += 3254;
              this.donate_count++;
          }
          else {
              alert("Ну харош а!?")
          }

      },
      reload(){
        this.currentCount = 0
        this.start = true
        this.gold = 1000
        this.crystal = 0
      }
  },
    mounted() {
        let audio = document.getElementById("dionSound");
        audio.volume = 0.1;
        audio.autoplay = true;
        audio.loop = true;
    }
}
</script>

<style>
  html, body {
    height: 100%;
    padding: 0 0 0 0;
    margin: 0 0 0 0;
  }
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  background: cadetblue url(./assets/dion.png);
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  background-blend-mode: hard-light, multiply, normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  height: 100%;
  padding: 0 0 0 0;
  margin: 0 0 0 0;
}
  .window {
    background:  url(./assets/bg.png);
    background-position: center;
    background-repeat: no-repeat;
    height: 100%;
    position: relative;
  }

  .window__workSpace_weapon {
    position: absolute;
    top:46%;
    left: 49.2%;
  }
  .window__workSpace_count{
    color: gold;
    position: absolute;
    top:50%;
    left: 49.4%;
  }
  .window__workSpace_info{
      color: gold;
      font-size: 12px;
      position: absolute;
      top:55.5%;
      left: 43%;
  }

  .window__footer {
    position: absolute;
    top:63.2%;
    left: 45.5%;
  }
  .window__footer__btn2 {
    padding-left: 9px;
  }
  .window__footer__btn1:hover, .window__footer__btn2:hover {
    transform: scale(1.03,1.03);
  }

  .btn__trade:hover {
      cursor: pointer;
  }
  .btn__trade{
      font-size: 9px;
  }
  .footer {
      position: absolute;
      bottom: 0;
      right:0;
  }
  .donate {
    cursor: pointer;
  }
  .weapon_4 {
      box-shadow: 0px 0px 30px 4px #bbecef;
  }
  .weapon_5 {
      box-shadow: 0px 0px 30px 5px #a9e3e6;
  }
  .weapon_6 {
      box-shadow: 0px 0px 30px 6px #9ae8ec;
  }
  .weapon_7 {
      box-shadow: 0px 0px 30px 7px #8ac1f1;
  }
  .weapon_8 {
      box-shadow: 0px 0px 30px 8px #4c78f3;
  }
  .weapon_9 {
      box-shadow: 0px 0px 30px 9px #1c4cf3;
  }
  .weapon_10 {
      box-shadow: 0px 0px 30px 10px #1c3bf3;
  }
  .weapon_11 {
      box-shadow: 0px 0px 30px 11px #f3afa9;
  }
  .weapon_12 {
      box-shadow: 0px 0px 30px 12px #f38886;
  }
  .weapon_13 {
      box-shadow: 0px 0px 30px 13px #f34f4d;
  }
  .weapon_14 {
      box-shadow: 0px 0px 30px 14px #f32f31;
  }
  .weapon_15 {
      box-shadow:  0px 0px 30px 15px #f31c1c;
  }
  .weapon_16 {
      box-shadow: 0px 0px 30px  16px red;
  }
</style>
