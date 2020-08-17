<template>
    <div class="outerWrapper">
        <div
            class="left arrow"
            v-if="pageNumber !== 0"
            @click="prevPage"
        >
            <Arrow color="#CCAE68"></Arrow>
        </div>
        <div class="list">
            <div class="innerWrapper">
                <div
                    v-for="(value, name) in this.getList"
                    :key="value"
                    :class="showCurrency(name)+' list-item'"
                    @click="setCurrency(name)"
                >
                    <span>{{name}}</span>
                </div>
            </div>
        </div>
        <div
            class="right arrow"
            v-if="pageNumber < pageCount -1"
            @click="nextPage"
        >
            <Arrow color="#CCAE68"></Arrow>
        </div>
    </div>
</template>

<script>
import Arrow from "@/components/Arrow";
export default {
    name: "NavBar",
    components: {
        Arrow
    },
    prop: {
        value: Object,
        items: Object,
    },
    data() {
        return {
            scroll: {
              value: 0,
              increment: 94
            },
            pageNumber: 0,
            size: 7
        }
    },
    methods: {
        changeSize() {
            let count = Math.floor(document.documentElement.clientWidth/100)
            this.size = (count > 7)
                ? 7 : (count < 3)
                    ? 3 : count
        },
        setCurrency(name) {
            this.$emit('input', name)
        },
        showCurrency(name) {
            if (this.$attrs.value === name)
                return 'selected'
        },
        nextPage() {
            this.pageNumber++
        },
        prevPage() {
            this.pageNumber--
        }
    },
    computed: {
        pageCount() {
            let length = Object.keys(this.$attrs.items).length,
                size = this.size

            return Math.ceil(length-size)
        },
        getList() {
            console.log(this.pageNumber)
            return Object.keys(this.$attrs.items).slice(this.pageNumber, this.pageNumber+this.size).reduce((result, key) => {
                result[key] = this.$attrs.items[key]
                return result
            }, {})
        }
    },
    created() {
        window.addEventListener("resize", this.changeSize)
    },
    mounted() {
        let count = Math.floor(document.documentElement.clientWidth/100)
        this.size = (count > 7)
            ? 7 : (count < 3)
                ? 3 : count
    }
}
</script>

<style scoped>
    .outerWrapper {
        position: relative;
        width: 100%;
        top: 1px;
    }
    .arrow {
        position: absolute;
        top: 15px;
        display: flex;
        cursor:pointer;
        padding: 10px;
        z-index: 5;
    }
    .left {
        left: 10px;
        transform: rotate(180deg);
    }
    .right {
        right: 10px;
    }
    .list {
        margin: 8px 40px 0;
    }
    .innerWrapper {
        display: flex;
        flex-direction: row;
        justify-content: space-around;
    }
    .innerWrapper div {
        display: inline-block;
        text-align: center;
        padding: 14px 30px 18px 30px;
        position: relative;
        cursor:pointer;
        color: #CCAE68;

        font-style: normal;
        font-weight: 500;
        font-size: 14px;
        line-height: 16px;
    }
    .selected {
        background-color: #FFFFFF;
        border-top-right-radius: 10px;
        border-top-left-radius: 10px;
        color: #2B2D33 !important;
    }
</style>