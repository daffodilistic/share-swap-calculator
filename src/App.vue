<template>
  <div id="app" class="container">
    <div class="row justify-content-center mt-4">
      <h6 class="small text-muted">
        Hand-crafted by
        <a
          href="https://www.twitter.com/daffodilistic"
          target="_blank"
        >@daffodilistic</a>
      </h6>
    </div>
    <ul class="nav nav-tabs" id="myTab" role="tablist">
      <li class="nav-item">
        <a
          class="nav-link"
          id="swap-tab"
          data-toggle="tab"
          href="#swap"
          role="tab"
          aria-controls="swap"
          aria-selected="false"
        >Swap Calculator</a>
      </li>
      <li class="nav-item">
        <a
          class="nav-link active"
          id="arbitrage-tab"
          data-toggle="tab"
          href="#arbitrage"
          role="tab"
          aria-controls="arbitrage"
          aria-selected="true"
        >Arbitrage Calculator</a>
      </li>
    </ul>
    <div class="tab-content" id="myTabContent">
      <div class="tab-pane fade" id="swap" role="tabpanel" aria-labelledby="swap-tab">
        <SwapCalculator />
      </div>
      <div class="tab-pane fade show active" id="arbitrage" role="tabpanel" aria-labelledby="arbitrage-tab">
        <ArbitrageCalculator />
      </div>
    </div>
  </div>
</template>

<script>
import ArbitrageCalculator from "./components/ArbitrageCalculator";
import SwapCalculator from "./components/SwapCalculator";

export default {
  name: "app",
  components: {
    ArbitrageCalculator,
    SwapCalculator
  },
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
