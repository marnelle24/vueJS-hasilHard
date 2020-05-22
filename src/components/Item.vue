<template>
    <div class="bg-white p-4">
        <center>
            <img :src="item.img_link" alt="..." class="max-w-full h-20 align-middle mt-4 mb-4" />
            <h1 class="md:text-lg font-bold">{{ item.name }}</h1>
            <h1 class="md:text-lg font-black text-green-400">{{ formatMoney(item.value) }}</h1>
            <div class="inline-flex mt-4 mb-4">
                <button @click="getSelectedItem(item, 'sell')" class="bg-red-400 hover:bg-red-500 text-gray-800 font-bold py-2 px-2 rounded-l">SELL</button>
                <input disabled onwheel="this.blur()" v-model="quantity" class="border rounded w-12 py-2 text-gray-700 leading-tight text-center" type="number">
                <button @click="getSelectedItem(item, 'buy')" class="bg-green-300 hover:bg-green-500 text-gray-800 font-bold py-2 px-2 rounded-r">
                    BUY
                </button>
            </div>
        </center>
    </div>
</template>

<script>
export default {
    name: 'Item',
    props:['item'],

    data() {
        return {
            quantity:0,
        }
    },

    methods: {
        formatMoney: function(amount) {
            return '$' + amount.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
        },

        getSelectedItem: function(item, ordertype) {

            if(ordertype === 'buy') {
                this.quantity++;
            }
            else {
                if(this.quantity == 0) {
                    return;
                }
                this.quantity--;  
            }

            this.$emit('handleItemSelected', [this.item, ordertype]);

        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .customDiv {
        cursor: pointer;
    }
    .customDiv:hover {
        background: #e2e8f0;
    }
</style>
