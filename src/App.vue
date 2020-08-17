<template>
    <div id="app">
        <div class="container">
            <div class="header">
                <h1>Курс {{base}} сегодня</h1>
                <NavBar v-model="base" :items="rates"></NavBar>
            </div>
            <p class="input">
                <input type="text" v-model="inputValue"/>
                <span> {{base}}</span>
            </p>
            <ListItem
                v-if="inputValue > 0"
                :value="{value: inputValue, base: base}"
                :items="rates"
            ></ListItem>
        </div>
    </div>
</template>

<script>
import NavBar from "@/components/NavBar";
import ListItem from "@/components/ListItem";

export default {
    name: 'App',
    components: {
        NavBar, ListItem
    },
    data() {
        return {
            base: 'EUR',
            rates: {},
            inputValue: null,
        }
    },
    methods: {
        async getRates() {
            let response = await fetch(
                    `https://api.exchangeratesapi.io/latest?base=${this.base}`
                ).then(function(response) {
                    if (response.status === 200) return response.json()
                    else this.showError()
                })
            if (response.base === this.base) {
                let EUR = (response.rates['EUR'] > 0)
                    ? response.rates['EUR']
                    : 1
                this.rates = Object.assign({EUR: EUR}, response.rates)
            }
        },
        showError() {
            console.log('error')
        }
    },
    watch: {
        'base': function() {
            this.getRates()
        },
        'inputValue': function() {
            this.inputValue = this.inputValue.replace(/[^\d]/g,'')
        }
    },
    created() {
        this.getRates()
    }
}
</script>

<style>
    @import url(https://allfont.ru/allfont.css?fonts=roboto-light);
    p {
        margin-block-start: 0;
        margin-block-end: 0;
    }
    #app {
        font-family: 'Roboto Light', arial;
        font-size: 18px;
        color: #2B2D33;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        margin-top: 60px;
        display: flex;
        justify-content: center;
    }
    .container {
        max-width: 720px;
        width: 100%;
    }
    .header h1 {
        font-size: 21px;
        font-weight: normal;
        line-height: 25px;
        margin: 30px 0 8px 24px;
    }
    .header {
        display: flex;
        flex-direction: column;
        width: 100%;
        background-color: #FFE782;
    }
    .input {
        margin: 29px 24px 23px 0;
        text-align: end;
        color: #B9B9B9;
    }
    .input input {
        border: none;
        background: none;
        outline: none;
        padding: 0 0 4px 0;

        width: 120px;
        font-size: 18px;
        text-align: end;
        border-bottom: 1px solid #D9D9D9;
    }
</style>
