<template>
  <div id="swap-calculator" class="container">
    <h3 class="my-2 text-center">Share Merger/Swap Calculator</h3>
    <br />
    <div class="col mx-auto">
      <div class="row">
        <div class="col">
          <h4 class="text-center">Merger/Swap Ratio</h4>
        </div>
        <div class="col">
          <input
            type="text"
            class="form-control"
            v-model="swapRatio"
            v-bind:placeholder="Math.random().toFixed(3)"
          />
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Board Lot Size</h4>
        </div>
        <div class="col">
          <input type="text" class="form-control" v-model="lotSize" placeholder="100" />
        </div>
      </div>
      <div class="row">
        <div class="mx-auto my-2">
          <input
            type="checkbox"
            class="form-check-input"
            v-model="allowOddLotsPurchase"
            id="allowOddLotsPurchase"
          />
          <label
            class="form-check-label"
            for="allowOddLotsPurchase"
          >Allow Pre-Swap Odd Lots Purchase</label>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <h4 class="text-center">Starting Pre-Swap Amount</h4>
        </div>
        <div class="col">
          <input type="text" class="form-control" v-model="startingAmount" placeholder="1100" />
        </div>
      </div>
      <hr />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Minimum Pre-Swap Amount</h4>
        </div>
        <div class="col">
          <input type="text" disabled class="form-control" v-model="preSwapAmount" />
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Post-Swap Amount</h4>
        </div>
        <div class="col">
          <input type="text" disabled class="form-control" v-model="postSwapAmount" />
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col col-md-4 mx-auto">
          <button
            type="button"
            v-on:click="calculate"
            class="btn btn-lg btn-block btn-primary"
          >Calculate</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SwapCalculator",
  components: {},
  data() {
    return {
      swapRatio: Math.random().toFixed(3),
      lotSize: 100,
      preSwapAmount: 0,
      postSwapAmount: 0,
      startingAmount: 1000,
      allowOddLotsPurchase: false
    };
  },
  methods: {
    calculate() {
      // console.log("Calculating...");

      this.preSwapAmount = 0;
      // We have to parseInt() to ensure type safety, until Vue v3 is released...
      var boardLotSize = parseInt(this.lotSize);
      var initialAmount = parseInt(this.startingAmount);
      var counter = 1;
      var found = false;

      if (this.allowOddLotsPurchase) {
        boardLotSize = 1;
      }

      while (!found) {
        var postSwapShares = Math.trunc(this.preSwapAmount * this.swapRatio);
        if (postSwapShares != 0 && postSwapShares % this.lotSize == 0) {
          this.postSwapAmount = postSwapShares;
          // console.log("postSwapAmount is " + this.postSwapAmount);
          found = true;
        } else {
          this.preSwapAmount = counter * boardLotSize + initialAmount;
          // console.log("preSwapAmount is " + this.preSwapAmount);

          counter++;
        }
      }
      // console.log("Done!");
    }
  }
};
</script>

<style></style>
