<template>
  <div class="col-sm-6 col-md-4 full-container">
    <div class="card bg-light mb-3" style="max-width: 18rem; min-width: 18rem;">
      <div class="card-header">{{ stock.name }}</div>
      <div class="card-body">
        <h5 class="card-title">(Price: {{ stock.price }})</h5>
        <div class="flex">
          <div class="pull-left">
            <input
              type="number"
              class="form-control"
              placeholder="Quantity"
              v-model.number="quantity"
              :class="{danger : insufficientFunds}"
              style="width: 110px"
            >
          </div>
          <div class="pull-right">
            <button
            class="btn btn-success"
            @click="buyStock"
            :disabled="insufficientFunds || quantity <= 0 || !Number.isInteger(quantity)"
            >{{ insufficientFunds ? 'NSF' : 'Buy' }}</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['stock'],
    data() {
      return {
        quantity: 0,
      }
    },
    computed: {
      insufficientFunds() {
        return this.quantity * this.stock.price > this.funds;
      },
      funds() {
        return this.$store.getters.funds;
      }
    },
    methods: {
      buyStock() {
        const order = {
          stockId: this.stock.id,
          stockPrice: this.stock.price,
          quantity: this.quantity
        };
        this.$store.dispatch('buyStock', order);
        this.quantity = 0;
      }
    }
  }
</script>

<style scoped>
  .flex {
    display: flex;
    justify-content: space-between
  }

  .card-header {
    background-color: lightgreen;
  }

  .full-container {
    margin-top: 50px;
  }

  .danger {
    border: 1px solid red;
  }
</style>