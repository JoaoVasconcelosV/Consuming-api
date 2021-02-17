<template>
    <div id="app">
        <div id="content">
            <h1 style="font-size: 48px; color: white">Preços do bitcoin</h1>

            <section v-if="errored">
                <p>Tivemos um erro e não conseguimos lhe oferecer os dados no momento. Por favor, tente novamente mais tarde.</p>
            </section>

            <section v-else>
                <div v-if="loading">Carregando...</div>

                <div
                v-else
                v-for="currency in info"
                :key="currency"
                class="currency">
                {{ currency.description }}:
                <span class="lighten">
                    <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
                </span>
                </div>

            </section>
        </div>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "App",
    data() {
        return {
            info: null,
            loading: true,
            errored: false,
        };
    },
    mounted() {
        axios
            .get("https://api.coindesk.com/v1/bpi/currentprice.json")
            .then((response) => {
                this.info = response.data.bpi;
            })
            .catch((error) => {
                console.log(error);
                this.errored = true;
            })
            .finally(() => (this.loading = false));
    },
    filters: {
        currencydecimal(value) {
            return value.toFixed(2);
        },
    },
};
</script>

<style>
body {
    margin: 0px
}
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    background-color: #c4c4c4;
    height: 100vh;
    width: 100vw;

    display: flex;
    align-items: center;
    justify-content: center;
}
#content {
    width: 916px;
    height: 639px;    

    color: #008000;
    font-size: 36px;

    background: rgba(0, 0, 0, 0.76);
    box-shadow: 33px 21px 20px 5px rgba(0, 0, 0, 0.25);
    border-radius: 10px;

    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}
</style>
