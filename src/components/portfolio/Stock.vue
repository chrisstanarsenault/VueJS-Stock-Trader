<template>
  <div class="col-sm-6 col-md-4 full-container" style="min-width: 18rem; max-width: 18rem;">
    <div class="card bg-light mb-3" style="max-width: 18rem; min-width: 18rem;">
      <div class="card-header">{{ stock.name }}</div>
      <div class="card-body">
        <h5 class="card-title">(Price: {{ stock.price }} | Quantity: {{ stock.quantity }})</h5>
        <div class="flex">
          <div class="pull-left">
            <input
              type="number"
              class="form-control"
              placeholder="Quantity"
              v-model.number="quantity"
              :class='{danger : insufficientStocks}'
              style="width: 100px"
            >
          </div>
          <div class="pull-right">
            <button
            class="btn btn-success"
            @click="sellStock"
            :disabled="insufficientStocks || quantity <= 0 || !Number.isInteger(quantity)"
            >{{insufficientStocks ? 'Not Enough' : 'Sell'}}</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import {mapActions} from 'vuex';

  export default {
    props: ['stock'],
    data() {
      return {
        quantity: 0,
      }
    },
    computed: {
      insufficientStocks() {
        return this.quantity > this.stock.quantity;
      }
    },
    methods: {
      ...mapActions({
        placeSellOrder:'sellStock'
      }),
      sellStock() {
        const order = {
          stockId: this.stock.id,
          stockPrice: this.stock.price,
          quantity: this.quantity
        };
        this.placeSellOrder(order);
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
    margin: 50px 100px 0 0;
  }

  .danger {
    border: 1px solid red;
  }
</style>