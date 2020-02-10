<template>
  <div id="app" class="container">
    <h3 class="my-2 text-center">Merger/Swap Arbitrage Calculator</h3>
    <div class="col mx-auto">
      <div class="d-flex justify-content-center">
        <div class="dropdown">
          <button
            class="btn btn-info dropdown-toggle"
            type="button"
            id="dropdownMenuButton"
            data-toggle="dropdown"
            aria-haspopup="true"
            aria-expanded="false"
          >Load Sample Data</button>
          <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
            <a class="dropdown-item" href="#" v-on:click="loadSample(1)">Sample 1 - "CCT/CMT"</a>
          </div>
        </div>
      </div>
    </div>
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
            v-model="swapData.swapRatio"
            v-bind:placeholder="0.5"
          />
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Cash Consideration</h4>
        </div>
        <div class="col">
          <input
            type="text"
            class="form-control"
            v-model="swapData.cashConsideration"
            v-bind:placeholder="0.10"
          />
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Buyer Swap Price</h4>
        </div>
        <div class="col">
          <input
            type="text"
            class="form-control"
            v-model="swapData.buyerSwapPrice"
            placeholder="1.0"
          />
        </div>
      </div>
      <hr />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Current Buyer Price</h4>
        </div>
        <div class="col">
          <input
            type="text"
            class="form-control"
            v-model="swapData.currentBuyerPrice"
            placeholder="1.3"
          />
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Current Offeree Price</h4>
        </div>
        <div class="col">
          <input type="text" class="form-control" v-model="swapData.currentOffereePrice" placeholder="1.2" />
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Offeree Entry Price</h4>
        </div>
        <div class="col">
          <input type="text" class="form-control" v-model="swapData.offereeEntryPrice" placeholder="1.0" />
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
  name: "ArbitrageCalculator",
  components: {},
  computed: {
    BROKERAGE_FEES() {},
    SAMPLE_DATA() {
      return {
        swapRatio: 0.72,
        cashConsideration: 0.259,
        buyerSwapPrice: 2.59,
        currentOffereePrice: 2.03,
        offereeEntryPrice: 1.8,
        currentBuyerPrice: 2.44
      };
    }
  },
  data() {
    return {
      swapData: {
        swapRatio: Math.random().toFixed(3),
        cashConsideration: Math.random().toFixed(3),
        buyerSwapPrice: 1.5,
        currentOffereePrice: 1.2,
        offereeEntryPrice: 1.0,
        currentBuyerPrice: 1.4
      }
    };
  },
  methods: {
    loadSample(sampleIndex) {
      // console.log("Sample loaded");
      this.swapData = this.SAMPLE_DATA;
    },
    calculate() {
      var currentSwapRatio =
          (this.swapData.swapRatio *
            (this.swapData.currentBuyerPrice /
              this.swapData.buyerSwapPrice));
      var impliedBuyerSharePrice =
        (this.swapData.offereeEntryPrice - this.swapData.cashConsideration) /
        currentSwapRatio;
      
      // console.log("currentSwapRatio is " + currentSwapRatio);
      console.log("impliedBuyerSharePrice is " + impliedBuyerSharePrice);

      // if impliedBuyerSharePrice > currentBuyerPrice, bad deal for child holders
      
    }
  }
};
</script>

<style></style>
