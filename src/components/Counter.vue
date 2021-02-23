<template>
  <div class="counter">
      <h1>
        {{timer}}
      </h1>
    </div>
</template>

<script>
export default {
    name: 'Counter',
    props: {
        countSeconds: {
            type:Number,
            default: 60
        }
    },
    data: function() {
        return {
            timer: this.countSeconds
        }
    },
    created: function() {
        alert("You have " + this.countSeconds + " seconds to solve the game. Are you ready?" );
    },
    watch: {
        timer: {
            handler(value) {
                if (value > 0) {
                    setTimeout(() => {
                        this.timer--;
                    }, 1000);
                } else {
                    let retVal = confirm("Game over! Start again?");
                    if ( retVal == true ) {
                        location.reload();
                    } else {
                        let el = document.getElementsByClassName("memory-game");
                        el[0].style.display = "none";
                        el = document.getElementById("game_over");
                        el.style.display = "block";
                    }
                }

            },
            immediate: true
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
