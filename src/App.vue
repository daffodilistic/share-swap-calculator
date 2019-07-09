<template>
  <div id="app" class="container">
    <h1 class="my-2 text-center">Share Merger/Swap Calculator</h1>
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
            placeholder="0.7942"
          />
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Board Lot Size</h4>
        </div>
        <div class="col">
          <input
            type="text"
            class="form-control"
            v-model="lotSize"
            placeholder="100"
          />
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
          <label class="form-check-label" for="allowOddLotsPurchase">
            Allow Pre-Swap Odd Lots Purchase
          </label>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <h4 class="text-center">Starting Pre-Swap Amount</h4>
        </div>
        <div class="col">
          <input
            type="text"
            class="form-control"
            v-model="startingPreSwapAmount"
            placeholder="1100"
          />
        </div>
      </div>
      <hr />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Minimum Pre-Swap Amount</h4>
        </div>
        <div class="col">
          <input
            type="text"
            disabled
            class="form-control"
            v-model="preSwapAmount"
          />
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Post-Swap Amount</h4>
        </div>
        <div class="col">
          <input
            type="text"
            disabled
            class="form-control"
            v-model="postSwapAmount"
          />
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col col-md-4 mx-auto">
          <button
            type="button"
            v-on:click="calculate"
            class="btn btn-lg btn-block btn-primary"
          >
            Calculate
          </button>
        </div>
      </div>
    </div>
    <div class="row justify-content-center mt-4">
      <h6 class="small text-muted">
        Hand-crafted by
        <a href="https://www.twitter.com/daffodilistic" target="_blank">
          @daffodilistic
        </a>
      </h6>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {},
  data() {
    return {
      swapRatio: 0.7942,
      lotSize: 100,
      preSwapAmount: 0,
      postSwapAmount: 0,
      startingPreSwapAmount: 1000,
      allowOddLotsPurchase: false
    };
  },
  methods: {
    calculate() {
      // console.log("Calculating...");

      this.preSwapAmount = 0;
      var preSwapLotSize = this.lotSize;
      var counter = this.startingPreSwapAmount;
      var found = false;

      if (this.allowOddLotsPurchase) {
        preSwapLotSize = 1;
      }

      while (!found) {
        this.preSwapAmount = counter * preSwapLotSize;
        // console.log("preSwapAmount is " + this.preSwapAmount);
        var postSwapShares = Math.trunc(this.preSwapAmount * this.swapRatio);
        if (postSwapShares != 0 && postSwapShares % this.lotSize == 0) {
          this.postSwapAmount = postSwapShares;
          // console.log("postSwapAmount is " + this.postSwapAmount);
          found = true;
        }
        counter++;
      }
      // console.log("Done!");
    }
  }
};
</script>

<style></style>
