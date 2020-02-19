<template>
  <div class="container">
    <transition name="fade" mode="out-in">
      <div class="insidebox" :key="hitTarget">
        <div class="speechbox">
          <div class="speech-bubble">
            <p v-if="hitTarget"><b>Project target has been reached!</b></p>
            <p v-else><transition name="fade" mode="out-in"><span :key="amountNeeded" class="bold">${{ amountNeeded }}</span></transition> still needed for this project</p>
          </div>
          <transition name="fade" mode="out-in">
            <div class="speechpoint" v-if="!hitTarget" v-bind:style="{ marginLeft: pointWidth }" :key="pointWidth"/>
          </transition>
        </div>
        <div class="blurbbox">
          <div class="progress">
            <div class="progress-bar progress-bar-animated" role="progressbar" v-bind:style="{ width: progressWidth}" v-bind:class="{orange: !hitTarget, green: hitTarget}" :aria-valuenow="amount" aria-valuemin="0" :aria-valuemax="amountNeeded"></div>
          </div>
          <div class="blurb">
            <p v-if="hitTarget">Goal met with <span class="bold">{{daysLeft}}</span> days left!</p>
            <p v-else><span class="daysleft bold">Only {{daysLeft}} days left</span> to fund this project.</p><br/>
            <p v-if="hitTarget">Thanks to the <span class="bold">{{donorAmount}}</span> donors who contributed to this project!</p>
            <p v-else>Join the <transition name="fade" mode="out-in"><span :key="donorAmount" class="bold">{{donorAmount}}</span></transition> other donors who have already supported this project. Every dollar helps.</p>
          </div>
          <div class="donationbox" v-if="!hitTarget">
            <input v-model="amount" type="number" value="50" min="0" step="1" data-number-to-fixed="2" data-number-stepfactor="100" class="form-control currency"/>
          </div>
          <div class="givenow" v-if="!hitTarget">
            <button v-on:click="addCash" type="submit" class="btn btn-primary greenbut bold">Give Now</button>
          </div>
          <div class="whygive" v-if="!hitTarget">
            <a src="#"><p class="blue"><i>Why give <transition name="fade" mode="out-in"><span :key="amount">${{amount}}</span></transition>?</i></p></a>
          </div>
        </div>
        <div class="savelater">
          <button type="submit" class="btn btn-primary greybut bold"><p v-if="!hitTarget">Save for later</p><p v-else>Learn more!</p></button>
        </div>
        <div class="tellfriends">
          <button type="submit" v-if="!hitTarget" class="btn btn-primary greybut bold">Tell your friends</button>
          <button type="submit" v-else v-on:click="resetAmount" class="btn btn-primary greybut bold">Restart</button>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
// import Logo from '~/components/Logo.vue'

export default {
  data: function() {
    return {
      maxAmount: 500,
      amountRaised: 333,
      daysLeft: 3,
      donorAmount: 42,
      amount: 50,
      hitTarget: false,
    }
  },
  computed: {
    amountNeeded: function () {
      return this.maxAmount - this.amountRaised;
    },
    progressWidth: function () {
      return ((this.amountRaised / this.maxAmount) * 100 + "%");
    },
    pointWidth: function () {
      return ((this.amountRaised / this.maxAmount) * 100 -2 + "%" )
    }
  },
  methods: {
    addCash: function () {
      this.amountRaised += this.amount;
      this.donorAmount += 1;
      if (this.amountRaised >= this.maxAmount) {
        this.hitTarget = true
      }
    },
    resetAmount: function () {
      Object.assign(this.$data, this.$options.data())
    }

  },
  components: {
    // Logo
  }
}
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css?family=Cabin&display=swap');

  $color-blue: #20A1D4;
  $color-green: #1CBC2C;
  $color-orange: #EF5F3C;
  $color-black: #424242;
  $color-grey: #777;
  $color-light-grey: #eaeaea;
  $padding: 15px;

  p {
    margin-bottom: 0;
  }

  .container {
    height: 100vh;
    width: 100vw;
    display: grid;
    justify-content: center;
    align-items: center;
    text-align: center;
    font-family: 'Cabin', sans-serif;
  }

  .insidebox {
    display: grid;
    grid-template-columns: 50% 50%;
    padding: $padding;
    grid-row-gap: 5%;
    transition: all .5s ease-out;
  }

  .speechbox {
    grid-column: 1/3;
    display: grid;
    grid-template-rows: 1fr 5%;
  }

  .speech-bubble {
    grid-column: 1/2;
    grid-row: 1/2;
    background-color: $color-black;
    color: $color-light-grey;
    border-radius: 4px;
    padding: $padding;
    position: relative;
  }

  .speechpoint {
    grid-row: 2/3;
    width: 0;
    border: 9px solid transparent;
    border-top-color: $color-black;
    border-bottom: 0;
    clip-path: polygon(50% 100%, 0 0, 100% 0);

  }

  .progress {
    grid-column: 1/3;
  }

  .orange {
    background-color: $color-orange;
  }

  .green {
    background-color: $color-green;
  }

  .blue {
    color: $color-blue;
  }

  .blurbbox {
    grid-column: 1/3;
    border: $color-light-grey 1px solid;
    display: grid;
    grid-template-columns: 50% 50%;
  }

  .blurb {
    grid-column: 1/3;
    padding: $padding;
  }

  .daysleft {
    color: $color-orange;
  }

  .donationbox {
    padding: $padding;
    align-self: center;
    justify-self: center;
    width: 80%;
    align-text: center;
  }

  .givenow {
    padding: $padding;
  }

  .whygive {
    grid-column: 1/3;
  }
  .greenbut {
    background-color: $color-green;
    color: $color-light-grey;
    border-color: $color-green;
  }
  .greenbut:hover {
    background-color: $color-light-grey;
    color: $color-green;
    border-color: $color-green;
  }
  .greenbut:target {
    background-color: $color-green;
    color: $color-light-grey;
    border-color: $color-green;
  }

  .greenbut:active {
    background-color: $color-green;
    color: $color-light-grey;
    border-color: $color-green;
  }

  .greenbut:focus {
    background-color: $color-green;
    color: $color-light-grey;
    border-color: $color-green;
  }

  .greybut {
    background-color: $color-light-grey;
    color: $color-grey;
    border-color: $color-grey;
  }

  .greybut:hover {
    background-color: $color-grey;
    color: $color-light-grey;
    border-color: $color-light-grey;
  }

  .bold {
    font-weight: bold;
  }

  .fade-enter-active, .fade-leave-active {
    transition: all .25s ease-out;
  }

  .fade-enter, .fade-leave-to  {
    opacity: 0;
  }

</style>
