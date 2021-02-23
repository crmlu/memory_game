<template>
    <div id="app">
        <counter></counter>
        <card v-bind:card-list="cards" v-on:cardClick="cardClickHandler($event)"></card>
        <div id="game_over">GAME OVER</div>
    </div>
</template>

<script>
    import Counter from './components/Counter.vue'
    import Card from './components/Card.vue'
    import CardsData from './json/data.json'

    export default {
        name: 'App',
        components: {
            Card,
            Counter
        }, 
        data: function() {
            return {
                openCard: null,
                clickCounter: 0,
                cards: CardsData,
            }
        },
        computed: {
            shuffledCards: function() {
                let currentIndex = this.cards.length;
                let temporaryValue, randomIndex;
                let cardArray = this.cards;

                while (currentIndex !== 0) {
                    randomIndex = Math.floor(Math.random() * currentIndex);
                    currentIndex -= 1;

                    temporaryValue = cardArray[currentIndex];
                    cardArray[currentIndex] = cardArray[randomIndex];
                    cardArray[randomIndex] = temporaryValue;
                }
                return cardArray;
            }
        },
        methods: {
            cardClickHandler: function (index) {
                this.cards[index].isFlipped = true;
                this.cards[index].isDisabled = true;

                if (this.openCard !== null) {
                    //no match
                    if (this.cards[index].name !== this.cards[this.openCard].name) {
                        setTimeout(() => {
                            this.cards[index].isDisabled = false;
                            this.cards[index].isFlipped = false;
                            this.cards[this.openCard].isDisabled = false;
                            this.cards[this.openCard].isFlipped = false;
                            this.openCard = null;
                        }, 1000);
                    }else {
                        //cards match
                        this.clickCounter = 0;
                        this.openCard = null;
                    }  
                } else {
                    //first iteration
                    this.openCard = index;
                } 
                this.clickCounter++;
            }
        },
        created: function (){
            this.cards = this.shuffledCards;
        }
    }
    </script>

    <style>
        * {
            padding: 0;
            margin: 0;
            box-sizing: border-box;
        }

        body {
            height: 100vh;
            display: flex;
            background-image: linear-gradient(120deg, #d4fc79 0%, #96e6a1 100%);
        }


        #app {
            width: 100%;
            padding: 7%;
        }
        .memory-game {
            width: 640px;
            height: 640px;
            margin: auto;
            display: flex;
            flex-wrap: wrap;
            perspective: 1000px;
        }

        .memory-card {
            width: calc(25% - 10px);
            height: calc(33.333% - 10px);
            margin: 5px;
            position: relative;
            transform: scale(1);
            transform-style: preserve-3d;
            transition: transform .5s;
            box-shadow: 1px 1px 1px rgba(0,0,0,.3);
        }

        .disable_card {
            pointer-events: none;
        }

        .memory-card:active {
            transform: scale(0.97);
            transition: transform .2s;
        }

        .memory-card.flip {
            transform: rotateY(180deg);
        }

        .front-face,
        .back-face {
            width: 100%;
            height: 100%;
            padding: 20px;
            position: absolute;
            border-radius: 5px;
            background: #FFF29E;
            backface-visibility: hidden;
        }

        .front-face {
            transform: rotateY(180deg);
        }

        .counter {
            position: absolute;
            top: 0px;
            right: 0px;
        }

        #game_over {
            display:none;
            text-align: center;
            font-size: 50px;
            color: red;
        }
    </style>
