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
          <input
            type="text"
            class="form-control"
            v-model="swapData.currentOffereePrice"
            placeholder="1.2"
          />
        </div>
      </div>
      <br />
      <div class="row">
        <div class="col">
          <h4 class="text-center">Offeree Entry Price</h4>
        </div>
        <div class="col">
          <input
            type="text"
            class="form-control"
            v-model="swapData.offereeEntryPrice"
            placeholder="1.0"
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
          >Calculate</button>
        </div>
      </div>
      <div id="result" class="row mt-3">
        <div class="alert alert-info col-6 mx-auto">
          <div class="row justify-content-center">
            <div class="my-auto">
              <h6 class="my-1">Implied Buyer Swap Price:</h6>
            </div>
            &nbsp;
            <div class="my-auto">
              {{ this.FORMATTER.format(this.swapData.currentBuyerPrice) }}
            </div>
          </div>
          <div class="row justify-content-center">
            <div class="my-auto">
              <h6 class="my-1">Implied Swap Ratio:</h6>
            </div>
            &nbsp;
            <div class="my-auto">
              {{ this.calculations.impliedSwapRatio.toFixed(3) }}
            </div>
          </div>
          <div class="row justify-content-center">
            <div class="my-auto">
              <h6 class="my-1">Implied Offeree Price:</h6>
            </div>
            &nbsp;
            <div class="my-auto">
              {{ this.FORMATTER.format(this.calculations.impliedOffereeSharePrice) }}
            </div>
          </div>
          <hr>
          <div class="row justify-content-center">
            <div class="my-auto">
              <h6 class="my-1 font-weight-bold">Implied Premium/Discount on Current Buyer Price:</h6>
            </div>
            &nbsp;
            <div class="my-auto">
              {{ this.FORMATTER.format(this.swapData.currentBuyerPrice - this.calculations.impliedBuyerSharePrice) }}
            </div>
          </div>
          <div class="row justify-content-center">
            <div class="my-auto">
              <h6 class="my-1 font-weight-bold">Implied Premium/Discount on Offeree Entry Price:</h6>
            </div>
            &nbsp;
            <div class="my-auto">
              {{ this.FORMATTER.format(this.swapData.offereeEntryPrice - this.calculations.impliedOffereeSharePrice) }}
            </div>
          </div>
          <div class="row justify-content-center">
            <div class="my-auto">
              <h6 class="my-1 font-weight-bold">Implied Premium/Discount on Current Offeree Price:</h6>
            </div>
            &nbsp;
            <div class="my-auto">
              {{ this.FORMATTER.format(this.swapData.currentOffereePrice - this.calculations.impliedOffereeSharePrice) }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import $ from "jquery";

export default {
  name: "ArbitrageCalculator",
  components: {},
  computed: {
    BROKERAGE_FEES() {
      return 0.0025;
    },
    FORMATTER() {
      const formatter = Object.freeze(new Intl.NumberFormat('en-SG', {
        style: 'currency',
        currency: 'SGD',
        minimumFractionDigits: 3
      }));

      return formatter;
    },
    SAMPLE_DATA() {
      return {
        swapRatio: 0.72,
        cashConsideration: 0.259,
        buyerSwapPrice: 2.59,
        currentBuyerPrice: 2.59,
        currentOffereePrice: 2.1238,
        offereeEntryPrice: 2.1238
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
      },
      calculations: {
        impliedBuyerSharePrice: 0,
        impliedSwapRatio: 0,
        impliedOffereeSharePrice: 0
      }
    };
  },
  mounted() {
    $("#result").hide(0);
  },
  methods: {
    loadSample(sampleIndex) {
      // console.log("Sample loaded");
      switch (sampleIndex) {
        default:
          break;
      }
      this.swapData = this.SAMPLE_DATA;
    },
    calculate() {
      var impliedSwapRatio =
        this.swapData.swapRatio *
        (this.swapData.currentBuyerPrice / this.swapData.buyerSwapPrice);
      var impliedBuyerSharePrice =
        (this.swapData.offereeEntryPrice - this.swapData.cashConsideration) /
        impliedSwapRatio;
      var impliedOffereeSharePrice = 
        this.swapData.currentOffereePrice - this.swapData.cashConsideration;

      console.log("impliedSwapRatio is " + impliedSwapRatio);
      console.log("impliedBuyerSharePrice is " + impliedBuyerSharePrice);
      console.log("currentBuyerPrice is " + this.swapData.currentBuyerPrice);

      this.calculations = {
        impliedSwapRatio: impliedSwapRatio,
        impliedBuyerSharePrice: impliedBuyerSharePrice,
        impliedOffereeSharePrice: impliedOffereeSharePrice
      };

      $("#result").hide(0, () => {
        $("#result").fadeIn();
      });
    }
  }
};
</script>

<style></style>
