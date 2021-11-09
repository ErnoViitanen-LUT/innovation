
<template>
  <div id="myApplication">
    
    <div id="faucet">   
      <img src="@/assets/faucet.jpg" />
      <div id="waterplaceholder">         
        <div v-html="drops" v-bind:class="{ active: isRunning }" class="rain front-row"></div>
        <div v-html="backdrops" v-bind:class="{ active: isRunning }" class="rain back-row"></div>
      </div>
    </div>
    <div class='textplaceholder'><button class='btn-one' v-on:click="makeItRain">
      <template v-if="isRunning">Close faucet</template>
      <template v-else>Let it rain</template>
    </button>
    <p><span>The amount of consumed water.</span></p>
    <p><span>You have spent <b>{{ (counter ).toFixed(3)}}</b> litres of water today.</span></p>
    <p><span>This equals <b>{{ euros.toFixed(2) }}</b> € for cold water,</span></p>
    <p><span>or <b>{{ heating.toFixed(2) }}</b> € for heated water.</span></p>
    <p><span>On average, this is monthly <b>{{ monthly.toFixed(2) }}</b> € for water.</span></p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',  
  data() {
    return {
      counter: 0,
      euros: 0,
      heating: 0,
      monthly: 0,
      isRunning: false,
      drops: [],
      backdrops: []
    }
  },
  mounted() {

    setInterval(() => {
      if(this.isRunning) {
        this.counter += 131 / 225;
        this.euros = this.counter / 1000 * 5
        this.heating = this.euros + (this.counter * 0.058 * 88 / 1000)
        this.monthly = (this.euros * 0.4 + this.heating * 0.6) * 30
      }
    }, 500)
  },
  methods: {
    makeItRain () {
      //clear out everything
        this.drops = []
        this.backdrops = []
      this.isRunning = !this.isRunning

      var increment = 0;
      var drops = "";
      var backdrops = "";

      if(this.isRunning){
        while (increment < 100) {
          //couple random numbers to use for various randomizations
          //random number between 98 and 1
          var randoHundo = (Math.floor(Math.random() * (98 - 1 + 1) + 1));
          //random number between 5 and 2

          var randoFiver = (Math.floor(Math.random() * (5 - 2 + 1) + 2));
   
          //increment
          increment += 1;
          //add in a new raindrop with various randomizations to certain CSS properties
          drops += '<div class="drop" style="left: ' + increment * 2 +   'px; bottom: ' + (randoFiver + randoFiver - 1 + 100) + '%; animation-delay: 0.' + randoHundo + 's; animation-duration: 0.3' + randoHundo + 's;"><div class="stem" style="animation-delay: 0.' + randoHundo + 's; animation-duration: 0.3' + randoHundo + 's;"></div><div class="splat" style="animation-delay: 0.' + randoHundo + 's; animation-duration: 0.3' + randoHundo + 's;"></div></div>';
          backdrops += '<div class="drop" style="left: ' + increment * 2 + 'px; bottom: ' + (randoFiver + randoFiver - 1 + 100) + '%; animation-delay: 0.' + randoHundo + 's; animation-duration: 0.3' + randoHundo + 's;"><div class="stem" style="animation-delay: 0.' + randoHundo + 's; animation-duration: 0.3' + randoHundo + 's;"></div><div class="splat" style="animation-delay: 0.' + randoHundo + 's; animation-duration: 0.3' + randoHundo + 's;"></div></div>';
        }

        this.drops.push(drops)
        this.backdrops.push(backdrops)
      }
    }
    
  }

}
</script>

<style>
html {
  height: 100%;
}
img {
  width: 300px;
}
body{
  height: 100%;
  margin: 0;
  overflow: hidden;
}

#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
#faucet {
  width: 20px;
  margin-left: 120px;
  margin-top: -120px;

}
span {
  font-family: 'Lato', sans-serif; font-size: 34px; font-weight: 200; line-height: 58px; margin: 0 0 58px; 
  
  -webkit-font-smoothing: antialiased;
}
span b {
  font-family: 'Lato', sans-serif; font-size: 74px; font-weight: 300; line-height: 58px; margin: 0 0 58px; 

  -webkit-font-smoothing: antialiased;
}
.textplaceholder {
  margin-top: -60px;
}
.drop {
  position: absolute;
  bottom: 100%;
  width: 15px;
  height: 120px;
  pointer-events: none;
  color: #2c3e50;
  animation: drop 0.3s linear infinite;

}

@keyframes drop {
  0% {
    transform: translateY(32vh);
  }
  75% {
    transform: translateY(90vh);
  }
  100% {
    transform: translateY(90vh);
  }
}


.stem {
  width: 2px;
  height: 60%;
  margin-left: 170px;
  background: linear-gradient(to bottom, rgba(22, 25, 185, 0.1), rgb(31, 48, 141));
  animation: stem 0.3s linear infinite;
}

@keyframes stem {
  0% {
    opacity: 1;
  }
  65% {
    opacity: 1;
  }
  75% {
    opacity: 0;
  }
  100% {
    opacity: 0;
  }
}

.splat {
  width: 25px;
  height: 10px;
  margin-left: 165px;
  border-top: 2px dotted rgba(37, 37, 44, 0.377);
  border-radius: 50%;
  opacity: 1;
  transform: scale(0);
  animation: splat 0.3s linear infinite;
  display: none;
}

body .splat {
  display: block;
}

@keyframes splat {
  0% {
    opacity: 1;
    transform: scale(0);
  }
  80% {
    opacity: 1;
    transform: scale(0);
  }
  90% {
    opacity: 0.3;
    transform: scale(1.5);
  }
  100% {
    opacity: 0;
    transform: scale(2);
  }
}

.btn-one {
  color: rgb(0, 0, 0);
  transition: all 0.3s;
  position: relative;
  padding: 0 10px 0 10px;
  min-width: 200px;

  font-family: 'Lato', sans-serif; font-size: 24px; font-weight: 400; line-height: 58px; margin: 0 0 0px; 
  
  -webkit-font-smoothing: antialiased;
}
.btn-one span {
  transition: all 0.3s;
}
.btn-one::before {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  opacity: 0;
  
  transition: all 0.3s;
  border-top-width: 1px;
  border-bottom-width: 1px;
  border-top-style: solid;
  border-bottom-style: solid;
  border-top-color: rgba(0, 0, 0, 0.5);
  border-bottom-color: rgba(255,255,255,0.5);
  transform: scale(0.1, 1);
}
.btn-one:hover span {
  letter-spacing: 2px;
}
.btn-one:hover::before {
  opacity: 1; 
  transform: scale(1, 1); 
}
.btn-one::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
  transition: all 0.3s;
  background-color: rgba(255,255,255,0.1);
}
.btn-one:hover::after {
  opacity: 0; 
  transform: scale(0.1, 1);
}
</style>