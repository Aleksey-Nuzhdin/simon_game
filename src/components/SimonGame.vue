<template>
  <div class="wrap">
    <h1 class="title">Simon Says</h1>
    <div class="game_wrap">
      <div class="game__feild">
        <div class="blue_btn game_btn"
          @click.prevent="gameClick(0)"
          :class="{activ:(playingMelody==0)}"
        ></div>
        <div class="red_btn game_btn"
          @click.prevent="gameClick(1)"
          :class="{activ:(playingMelody==1)}"
        ></div>
        <div class="yellow_btn game_btn"
          @click.prevent="gameClick(2)"
          :class="{activ:(playingMelody==2)}"
        ></div>
        <div class="green_btn game_btn"
          @click.prevent="gameClick(3)"
          :class="{activ:(playingMelody==3)}"
        ></div>
      </div>
      <div class="game__info">
        <div class="raund">
          <h3 class="raund__title">Раунд: {{roundLevel}}</h3>
          <span class="loss_meseg"
            v-if="isLoss"
          >{{lossMesseg}} {{roundLevel}}</span>
          <button class="btn_starn_game"
            @click.prevent="startGame()"
          >Начать</button>
        </div>
        <div class="game__setup">
          <div class="group__setup_btn">
            <input type="radio" name='speedSetup' id='speed_lvl_1' class="speed__setup" 
              value="1500"
              v-model="speedLevel"
            >
            <label for="speed_lvl_1" class="label__setup">Легкий</label>
          </div>
          <div class="group__setup_btn">
            <input type="radio" name='speedSetup' id='speed_lvl_2' class="speed__setup"
              value="1000"
              v-model="speedLevel"
            >
            <label for="speed_lvl_2" class="label__setup">Средний</label>
          </div>
          <div class="group__setup_btn">
            <input type="radio" name='speedSetup' id='speed_lvl_3' class="speed__setup"
              value="400"
              v-model="speedLevel"
            >
            <label for="speed_lvl_3" class="label__setup">Сложный</label>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data:()=>({
    isGameStart: false,
    isMelodyPlaying: false,
    isLoss: false,
    speedLevel: '1500',
    lossMesseg: '',
    roundLevel: 0,
    countNote: 4,
    melody:[],
    melodyGamer:[],
    playingMelody: -1,
  }),
  methods:{
    startGame(){
      if(this.isGameStart) return
      this.isGameStart = true

      this.isLoss = false
      this.lossMesseg = ''
      this.melodyGamer = []

      this.addNoteInMelody()
      this.isMelodyPlaying = true
      this.playMelody([...this.melody])
      
    },
    addNoteInMelody(){
      this.melody.push(
        Math.floor( Math.random() * this.countNote )
      )
    },
    gameClick(note){
      if(this.isMelodyPlaying) return 
      this.playingMelody = note
      setTimeout(()=>{this.playingMelody = -1}, 300)

      if(!this.isGameStart) return

      this.melodyGamer.push(note)

      if(!this.checkMelody()){
          this.gameLoss()
          return
      }
      
      if(this.melody.length == this.melodyGamer.length){
        
        this.roundLevel++
        this.isGameStart = false
        setTimeout(this.startGame, 1500) 
      }

    },
    gameLoss(){
      this.isLoss = true
      this.lossMesseg = 'Вы проиграли со счётом'
      this.isGameStart = false
    },
    checkMelody(){
      const result = this.melodyGamer.every((element, index)=>{
        console.log(element);
        return element === this.melody[index]   
      })

      return result
    },
    playMelody(melody){
      if(melody.length > 0){
        this.playingMelody = melody.shift()


        setTimeout(()=>{this.playingMelody = -1}, +this.speedLevel - 100)
        setTimeout(this.playMelody, +this.speedLevel, melody)
        return
      }
      this.playingMelody = -1
      this.isMelodyPlaying = false
      return
    },
  },
}
</script>

<style scoped lang="scss">
.wrap{
  display: flex;
  height: 100%;
  width: 100%;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.title{
  font-size: 45px;
  color: #333;
  font-weight: bold;
}
.loss_meseg{
  display: block;
}
.raund__title{
  font-size: 25px;
  color: #333;
  font-weight: bold;
}
.game__info{
  margin-left: 20px;
}
.btn_starn_game{
  height: 50px;
  width: 100px;
  margin-top: 10px;
  font-size: 20px;
  font-weight: bold;
  border-radius: 10px;
  background-color: rgb(69, 56, 255);
  color: rgb(245, 244, 244);
}
.game_wrap{
  display: flex;
  margin-top: 40px;
}
.game__feild{
  display: flex;
  flex-wrap: wrap;
  height: 400px;
  width: 400px;
  border: 2px solid #555;
  border-radius: 50%;
  overflow: hidden;
}

.game_btn{
  width: 50%;
  height: 50%;
  opacity: .5;

  &.activ{
    opacity: 1;
  }
}
.blue_btn{
 background-color: rgb(0, 4, 255);
}
.red_btn{
 background-color: rgb(255, 0, 0);
}
.yellow_btn{
  background-color: rgb(255, 238, 0);
}
.green_btn{
  background-color: rgb(21, 255, 0);
}
.game__setup{
  margin-top: 10px;
}
</style>
