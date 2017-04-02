<template>
  <div id="app">
    <div class="container">
      <div class="columns">
        <div class="column">

          <div class="field">
            <label class="label">Prizes (in order)</label>
            <p class="control">
              <textarea :class="['textarea',inputClasses]" placeholder="Enter prizes here" v-model="prizeInput"></textarea>
            </p>
          </div>

        </div>
        <div class="column">
          
          <div class="field">
            <label class="label">Participants</label>
            <p class="control">
              <textarea :class="['textarea',inputClasses]" placeholder="Enter participants here" v-model="participantInput"></textarea>
            </p>
          </div>
        </div>

        <div class="column">
          
          <div>ParticipantList: {{ participantList }}</div>
          <div>PrizeList: {{ prizeList }}</div> 
          <div>Current Prize: {{currentPrize}}</div> 
          <div>Current Winner: {{currentWinner}}</div> 

        </div>
      </div>

      <div class="box has-text-centered">
        <p class="heading" v-if="started">Prize #{{prizeNum}}</p>
        <h1 class="title" v-if="started"><strong>{{currentPrize}}</strong></h1>
        <p v-if="winnerRevealed">goes to</p>
        <h1 class="title" v-if="winnerRevealed">{{currentWinner}}</h1>
        <button class="button is-success is-large" @click="startRaffle" v-if="!started">Start!</button>
        <button class="button is-primary" @click="pickWinner" v-if="!winnerRevealed && started">Raffle!</button>
        <button class="button" @click="repickWinner" v-if="winnerRevealed && started">Choose New Winner</button>
        <button class="button is-success" @click="nextItem" v-if="winnerRevealed && started">Next</button>
      </div>

      <div class="columns">

        <div class="column">
          <div class="box">
            <div class="level" v-for="prize in log">
              {{ prize.item }} -> {{ prize.winner }}
            </div>
          </div>
        </div>

        <div class="column">
          
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data() {
      return {
        prizeInput: 'Processor\nKeyboard\nMousepad\nMouse',
        participantInput: 'Price\nClaire\nMark\nJoe\nJustin\nJason\nBass',
        winnerRevealed: false,
        started: false,
        prizeList: [],
        participantList: [],
        log: [],
        currentPrize: '',
        currentWinner: '',
        prizeNum: 0
      }
    },
    methods: {
      startRaffle() {
        this.prizeList = this.prizeInput.split('\n')
        this.participantList = this.participantInput.split('\n')
        
        this.started = true;
        this.nextItem()
      },
      pickWinner() {
        //Get random winner
        this.currentWinner = this.participantList.splice(Math.floor(Math.random()*this.participantList.length),1)[0]
        this.winnerRevealed = true
      },
      repickWinner() {
        //Get random winner
        this.currentWinner = this.participantList.splice(Math.floor(Math.random()*this.participantList.length),1)[0]
      },
      nextItem() {
        this.prizeNum++
        this.currentPrize = this.prizeList.shift()
        log.push({
          item: this.currentPrize,
          winner: this.currentWinner
        })

        this.winnerRevealed = false
      }
    },
    computed: {
      
      inputClasses() {
        return {
          'is-disabled': this.started
        }
      }
    }
  }

</script>

<style>

</style>