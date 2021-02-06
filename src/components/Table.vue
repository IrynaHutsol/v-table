<template>
  <div>
    <button @click="getData" class="btn-get-data">Get data</button>
    <div v-if="loading">
        Loading...
    </div>
    <div class="table" v-if="this.newData.length > 0">
        <div class="table-header">
            <div @click="sort" class="row-header">Stock</div>
            <div class="row-header">Current</div>
            <div class="row-header">Change</div>
        </div>
        <div class="table-body">
            <div class=table-row v-for="(item, i) in this.newData" :key="i">
                <div class="row">
                    {{item.stocks}}
                </div>
                <div class="row">
                    {{item.current.toFixed(2)}}
                </div>
                <div class="row" :class="{plus:item.change > 0, minus:item.change < 0}">
                    {{item.change.toFixed(2).includes('-') ? item.change.toFixed(2) : '+' + item.change.toFixed(2)}}
                </div>
            </div>
        </div>
    </div>
    <div v-if="noData">
        No data
    </div> 
  </div>
</template>

<script>
import { simulateAsyncReq } from '@/plugins/getDataFunc.js'
import { payload } from '@/mocData/index.js'

export default {
    data () {
        return {
            data: {},
            noData: false,
            loading: false,
            newData: [],
            count: 0
        }
    },
    methods: {
        async getData() {
            try {
                this.newData = []
                this.loading = true
                this.noData = false
                this.data = await simulateAsyncReq(payload)
                this.loading = false
                this.noData = false

                for (let i = 0; i < Object.keys(this.data).length; i++) {
                    this.newData.push({
                        stocks: this.data.stocks[i],
                        current: this.data.current[i],
                        change: this.data.current[i] - this.data.start[i]
                    });
                }
            }
            catch (error) {
                this.noData = true
                this.loading = false
            }
        },
        sort() {
            this.count++ 
            if (this.count & 1){
                this.newData.sort((a,b) => a.stocks.localeCompare(b.stocks))
            }else {
                this.newData.reverse();
            }
        }
    }
}
</script>

<style lang="scss">
.btn-get-data {
    height: 40px;
    width: 120px;
    background: #eaeaea;
    border: 1px solid #e0dede;
    color: #565353;
    margin-bottom: 20px;
    font-size: 15px;
    cursor: pointer;
}
.btn-get-data:focus {
    outline: none !important;
}
.table {
    max-width: 350px;
    margin: 0 auto;
    border: 1px solid #e0dede;
}
.table-header {
    display: flex;
    justify-content: space-evenly;
    height: 40px;
    align-items: center;
    background: #eaeaea;
    border-bottom: 1px solid #e0dede;
    font-weight: 600;
}
.table-row {
    display: flex;
    justify-content: space-evenly;
    height: 30px;
    align-items: center;
    border-bottom: 1px solid #e0dede;
}
.table-row:last-child {
    border-bottom: none;
}
.plus {
    color: #62e262;
}
.minus {
    color: red;
}
.row, .row-header {
    width: 33%;
}
.row-header:first-child {
    cursor: pointer;
}
</style>