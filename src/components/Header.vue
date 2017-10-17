<template>
    <nav class="navbar navbar-expand-md navbar navbar-dark bg-dark">
        <router-link to="/" class="navbar-brand">Stock Trader</router-link>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav mr-auto">
                <router-link tag="li" to="/portfolio" class="nav-item" activeClass="active"><a class="nav-link">Portfolio</a></router-link>
                <router-link tag="li" to="/stocks" class="nav-item" activeClass="active"><a class="nav-link">Stocks</a></router-link>
            </ul>
            <ul class="navbar-nav navbar-right">
                <li class="nav-item">
                    <a class="nav-link" href="#" @click="endDay">End Day <span class="sr-only">(current)</span></a>
                </li>
                <li class="nav-item dropdown">
                    <a
                            class="nav-link dropdown-toggle"
                            href="http://example.com"
                            id="navbarDropdownMenuLink"
                            data-toggle="dropdown"
                            aria-haspopup="true"
                            aria-expanded="false"
                    >Save & Load</a>
                    <div class="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
                        <a class="dropdown-item" href="" @click="saveData">Save Data</a>
                        <a class="dropdown-item" href="" @click="loadData">Load Data</a>
                    </div>
                </li>
            </ul>
            <strong class="navbar-text">Funds: {{ funds | currency }}</strong>
        </div>
    </nav>
</template>

<script>
    import { mapActions } from 'vuex';

    export default {
        computed: {
            funds() {
                return this.$store.getters.funds;
            }
        },
        methods: {
            ...mapActions({
                randomizeStocks: 'randomizeStocks',
                fetchData: 'loadData'
            }),
            endDay() {
                this.randomizeStocks();
            },
            saveData() {
                const data = {
                    funds: this.$store.getters.funds,
                    stockPortfolio: this.$store.getters.stockPortfolio,
                    stocks: this.$store.getters.stocks
                };
                this.$http.put('data.json', data);
                console.log(data);
            },
            loadData() {
                this.fetchData();
            }
        }
    }
</script>