<template>
    <div class="body">
        <div class="list">
            <div
                v-for="(value, name) in paginatedInputValue"
                :key="name"
                class="card"
            >
                <p class="cardHeader">
                    <span>{{inputValue.value}} </span>
                    {{inputValue.base}} =
                </p>
                <p class="cardBody">
                    <span>{{resultValue(value)}}</span>
                    {{name}}
                </p>
            </div>
        </div>
        <div class="pagination">
            <button
                class="prev"
                @click="prevPage"
                :disabled="pageNumber === 0"
            >
                <Arrow :color="(pageNumber === 0) ? '#787878' : '#282828'"></Arrow>
                Назад
            </button>
            <button
                class="next"
                @click="nextPage"
                :disabled="pageNumber >= pageCount -1"
            >
                Далее
                <Arrow :color="(pageNumber >= pageCount -1) ? '#787878' : '#282828'"></Arrow>
            </button>
        </div>
    </div>
</template>

<script>
import Arrow from "@/components/Arrow";
export default {
    name: "ListItem",
    components: {
        Arrow
    },
    props: {
        items: Object,
        value: Object
    },
    data() {
        return {
            pageNumber: 0,
            size: 4
        }
    },
    updated() {
        if (this.pageNumber === this.pageCount-1) {
            let elementList = document.querySelectorAll('.body .list>div')
            if (elementList.length < 3)
                elementList.forEach(function(element) {
                    element.style.maxHeight = '132px';
                })
        }
    },
    methods: {
        changeSize() {
            if (document.documentElement.clientWidth < 600) this.size = 2
            else this.size = 4
        },
        resultValue(val) {
            return (val*this.inputValue.value).toFixed(2)
        },
        nextPage() {
            this.pageNumber++
        },
        prevPage() {
            this.pageNumber--
        }
    },
    computed: {
        inputValue() {
            return this.value
        },
        rates() {
            return this.items
        },
        pageCount() {
            let length = Object.keys(this.rates).length,
                size = this.size

            return Math.ceil(length/size)
        },
        paginatedInputValue() {
            let start = this.pageNumber * this.size,
                end = start + this.size

            return Object.keys(this.rates).slice(start, end).reduce((result, key) => {
                result[key] = this.rates[key]
                return result
            }, {})
        }
    },
    created() {
        window.addEventListener("resize", this.changeSize)
    }
}
</script>

<style scoped>
    .body {
        margin: 5px 15px;
    }
    .list {
        display: flex;
        flex-wrap: wrap;
        min-height: 372px;
    }
    .card {
        font-weight: 300;
        font-size: 24px;
        line-height: 28px;

        display: inline-flex;
        flex-direction: column;
        margin: 9px;
        padding: 18px 30px;
        width: calc(50% - 78px);

        border-radius: 12px;
        box-shadow: 0px 3px 6px rgba(157, 157, 157, 0.16);
    }
    .cardHeader {
        color: #B9B9B9;
        margin-bottom: 19px;
    }
    .cardHeader span {
        color: #2B2D33;
    }
    .cardBody span {
        font-size: 48px;
        line-height: 56px;
    }
    .pagination {
        display: flex;
        flex-direction: row;
        justify-content: center;
        margin: 41px 0 9px 0;
    }
    .pagination button {
        padding: 10px 19px 8px 25px;
        font-size: 14px;
        text-transform: uppercase;
        cursor: pointer;
        background: #FFFFFF;
        border: none;
        box-shadow: 0px 5px 6px rgba(157, 157, 157, 0.16);
        border-radius: 8px;
    }
    .pagination button:disabled {
        background-color: #EFEFEF;
        color: #787878;
        cursor: default;
    }
    .pagination .prev {
        margin-right: 5px;
    }
    .pagination .next {
        margin-left: 5px;
    }
    .pagination .prev svg {
        margin-right: 10px;
        transform: rotate(180deg);
    }
    .pagination .next svg {
        margin-left: 10px;
    }
    @media (max-width: 600px) {
        .card {
            width: calc(100% - 18px);
        }
    }
</style>