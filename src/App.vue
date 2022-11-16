<template>
  <div id="app">
    <div class="info">
      <span class="moves">Hamle: {{moves}}</span>
      <button v-if="isGameFinished" @click="gameFinish" class="gameFinished">Yeniden oyna</button>
    </div>
    <div class="wrapper">
      <div @click="action(key)" :key="item.iconName+key" v-for=" (item,key) in cards" class="boxes"
        :class="{show:item.isCardOpened}">
        <div class="hider">
          <i :class="'fa fa-'+item.iconName"></i>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        moves: 0,
        card: '',
        sameCardController: null,
        cards: [{
            iconName: 'person',
            isCardOpened: false
          },
          {
            iconName: 'smile',
            isCardOpened: false
          },
          {
            iconName: 'tree',
            isCardOpened: false
          },
          {
            iconName: 'heart',
            isCardOpened: false
          },
          {
            iconName: 'car',
            isCardOpened: false
          }, {
            iconName: 'camera',
            isCardOpened: false
          },
          {
            iconName: 'house',
            isCardOpened: false
          },
          {
            iconName: 'cloud',
            isCardOpened: false
          }
        ]
      }
    },
    computed: {
      isGameFinished() {
        return this.cards.every(item => item.isCardOpened)
      }
    },
    methods: {
      cardHandler() {
        this.cards.push(...JSON.parse(JSON.stringify(this.cards)))
      },
      shuffleHandler() {
        this.cards = this.cards.map(function (n) {
            return [Math.random(), n]
          })
          .sort().map(function (n) {
            return n[1]
          });
      },
      action(cardIndex) {
        if (this.card == '') {
          this.sameCardController = cardIndex
        }
        const isCloneCardFound = this.cards.filter(item => item.iconName == this.cards[cardIndex].iconName && item
          .isCardOpened == true)
        if (isCloneCardFound.length == 2) {
          return;
        }
        if (this.card != '' && this.sameCardController == cardIndex) {
          return;
        }
        this.moves++
        if (this.card == '') {
          this.cards[cardIndex].isCardOpened = true
          this.card = this.cards[cardIndex].iconName
        } else {
          if (this.card == this.cards[cardIndex].iconName) {
            this.cards[cardIndex].isCardOpened = true
            this.card = ''
          } else {
            this.cards[cardIndex].isCardOpened = true
            const firstClickedCard = this.cards.find(item => item.iconName == this.card && item.isCardOpened == true)
            this.card = ''
            setTimeout(() => {
              firstClickedCard.isCardOpened = false, this.cards[cardIndex].isCardOpened = false
            }, 500)
          }
        }
      },
      gameFinish() {
        this.cards = this.cards.map(item => {
          return {
            iconName: item.iconName,
            isCardOpened: false
          }
        })
        this.moves = 0
        this.shuffleHandler()
      }
    },
    created() {
      this.cardHandler()
      this.shuffleHandler()
    },
  }
</script>

<style>
  * {
    box-sizing: border-box;
    font-family: sans-serif;
    -webkit-text-stroke: 1px black;
    color: white;
  }

  #app {
    display: grid;
    justify-items: center;
    height: 100%;
    align-content: center;
  }

  html,
  body {
    margin: 0;
    height: 100%;
    background: linear-gradient(325deg, #6F00FC 0%, #FC7900 50%, #FCC700 100%);
  }

  .wrapper {
    background: linear-gradient(135deg, #6F00FC 0%, #FC7900 50%, #FCC700 100%);
    width: 555px;
    padding: 15px;
    border-radius: 5px;
  }

  .boxes {
    width: calc(25% - 20px);
    height: 100px;
    border-radius: 5px;
    background-color: rgb(32, 32, 32);
    margin: 10px;
    float: left;
    color: white;
    font-size: 50px;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    perspective: 1000px;
    transition: transform 0.6s;
    transform-style: preserve-3d;
  }

  .show {
    transform: rotateY(180deg);
  }

  .hider {
    transform: rotateY(180deg);
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
  }

  .info {
    text-align: center;
    font-size: 40px;
    display: grid;
    margin-bottom: 5px;
  }

  .gameFinished {
    background-color: #aa60c3;
    color: white;
    border: 0;
    font-size: 20px;
    padding: 10px;
    border-radius: 5px;
    width: 150px;
  }

  @media only screen and (max-width: 600px) {
    .wrapper {
      width: 90% !important;

    }

    .boxes {
      height: 80px !important;
      font-size: 35px !important;
    }

  }
</style>