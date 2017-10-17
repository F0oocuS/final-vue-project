<template>
    <div class="col-sm-6 col-lg-4 col-xxl-3">
        <div class="card border-success mb-3">
            <div class="card-header bg-success">
                {{ stock.name }}
                <small>(Price: {{ stock.price }})</small>
            </div>
            <div class="card-body text-success">
                    <input
                            type="number"
                            class="form-control"
                            placeholder="Quantity"
                            v-model="quantity"
                            :class="{danger: insufficientFunds}">
                <hr>
                <div>
                    <button
                            class="btn btn-success"
                            @click="buyStock"
                            :disabled="insufficientFunds || quantity <= 0"
                        >{{ insufficientFunds ? 'Insufficient Funds' : 'Buy' }}</button>
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
                quantity: 0
            }
        },
        computed: {
            funds() {
                return this.$store.getters.funds;
            },
            insufficientFunds() {
                return this.quantity * this.stock.price > this.funds;
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
    .danger {
        border: 1px solid red;
    }
</style>
