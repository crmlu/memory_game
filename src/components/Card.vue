<template>
    <section class="memory-game">
        <div v-for="(card, index) in cardList" v-on:click="clickCard(index)" v-bind:id="'card_' + card.id" v-bind:key="index" class="memory-card" v-bind:class="{ flip: card.isFlipped, disable_card: card.isDisabled }" :data-framework="card.name">
            <img class="front-face" v-bind:src="card.imgUrl" v-bind:alt="card.desc" />
            <img class="back-face" src="img/js-badge.svg" alt="JS Badge" />
        </div>
    </section>
</template>

<script>
    export default {
        name: 'Card',
        props: {
            cardList: {
            type: Array,
            required: true
            },
        },
        methods: {
            clickCard: function(index) {
                this.$emit('cardClick', index);
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
