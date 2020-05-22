<template>
    <div id="app">
        <div class="flex justify-center bg-gray-200 pb-5 pt-4">
            <h1 class="text-green text-6xl uppercase font-black">#HasilHard</h1>
        </div>

         <div class="flex justify-center bg-gray-200 pb-10 pt-2">
            <h1 v-if="is_identityNotEmpty" class="text-2xl">HOW WILL YOU SPEND THE MONEY OF <strong class="uppercase">{{ selectedIdentity.name }}</strong>?</h1>
            <h1 v-else class="text-2xl">CHOOSE YOUR IDOL</h1>
        </div>

        <div v-if="is_identityNotEmpty" class="flex flex-wrap justify-center bg-gray-200 pb-10">
            <div class="w-6/12 sm:w-4/12 px-4">
                <h1 class="md:text-4xl xl:text-black font-extrabold bg-green-400 mb-2">{{ selectedIdentity.name }}</h1>
                <img :src="selectedIdentity.img_link" alt="..." class="shadow-lg rounded-full max-w-full h-auto align-middle border-none" />
                <h1 class="md:text-4xl xl:text-black font-extrabold bg-green-400 mt-2">{{ formatMoney(showBalance) }} {{ showBalance>0 ? 'left' : '' }}</h1>
            </div>
        </div>

        <your-identity v-else :identity_data="identities" v-on:getIdentity="getSelectedIdentity"/>

        <h1 v-if="seledtedItems.lenght" class="md:text-2xl font-bold bg-gray-200 pl-10 py-5"> Your purchases</h1>
        
        <div class="grid grid-cols-6 gap-4 justify-center bg-gray-200 px-10">
            <div v-for="(item,key) in seledtedItems" :key=key class="bg-white p-4" v-show="item.qnty>0">
                <img :src="item.img_link" alt="..." class="m-auto w-20 h-auto align-middle border-none" />
                <span class="text-xl font-bold pt-2 pb-0">{{ item.name }}</span> 
                    <br /> {{ formatMoney(item.value) }} x {{ item.qnty }}
                    <br />{{ formatMoney(item.value*item.qnty) }}
            </div>
        </div>

        <hr class="border-2" />

        <div v-if="is_identityNotEmpty" class="grid xl:grid-cols-4 bg-gray-200 gap-4 pt-5 px-10 py-10">
            <item  v-for="(item, key) in list_items" :key=key :item="item" v-on:handleItemSelected="calculate_items"></item>
        </div>

        <!-- <h4>You have selected {{ selectedIdentity.name }} with {{ selectedIdentity.networth }}</h4> -->

    </div>
</template>

<script>
import YourIdentity from './components/YourIdentity.vue'
import Item from './components/Item.vue'

export default {
    name: 'App',
    components: {
        YourIdentity,
        Item
    },
    data() {
        return {
            selectedIdentity:[],
            is_identityNotEmpty: false,

            list_items: [
                { id:0,
                  name: 'Town House',
                  value: 180000,
                  img_link: 'https://neal.fun/spend/minified/townhouse.jpg'
                },
                { id:1,
                  name: 'Luxury Yatch',
                  value: 10000000,
                  img_link: 'https://neal.fun/spend/minified/yacht.jpg'
                },
                { id:2,
                  name: 'Mona Lisa',
                  value: 780000,
                  img_link: 'https://neal.fun/spend/minified/monalisa.jpg'
                },
                { id:3,
                  name: 'Mansion',
                  value: 100000,
                  img_link: 'https://neal.fun/spend/minified/mansion.jpg'
                },
                { id:4,
                  name: 'Helicopter',
                  value: 175000,
                  img_link: 'https://neal.fun/spend/minified/helicopter.jpg'
                },
                { id:5,
                  name: 'Bar of Gold',
                  value: 500000,
                  img_link: 'https://neal.fun/spend/minified/barofgold.jpg'
                },
                { id:6,
                  name: 'Cruise Ship',
                  value: 1200000,
                  img_link: 'https://neal.fun/spend/minified/cruiseship.jpg'
                },
                { id:7,
                  name: 'Passenger Jet',
                  value: 150000000,
                  img_link: 'https://neal.fun/spend/minified/passengerjet.jpg'
                },
                { id:8,
                  name: 'Skyscraper',
                  value: 50000000,
                  img_link: 'https://neal.fun/spend/minified/skyscraper.jpg'
                },
                { id:9,
                  name: 'Beach House',
                  value: 5000000,
                  img_link: 'https://neal.fun/spend/minified/beachhouse.jpg'
                },
                { id:10,
                  name: 'Rolex',
                  value: 15000,
                  img_link: 'https://neal.fun/spend/minified/rolex.jpg'
                },
                { id:11,
                  name: 'Diamond Ring',
                  value: 10000,
                  img_link: 'https://neal.fun/spend/minified/diamondring.jpg'
                },
                { id:12,
                  name: 'Tesla',
                  value: 70000,
                  img_link: 'https://neal.fun/spend/minified/tesla.jpg'
                },
                { id:13,
                  name: 'Ferrari',
                  value: 200000,
                  img_link: 'https://neal.fun/spend/minified/ferrari.jpg'
                },
                { id:14,
                  name: 'Lamborghini',
                  value: 20000,
                  img_link: 'https://neal.fun/spend/minified/lamborghini.jpg'
                },
                { id:15,
                  name: 'F16 Figther Jet',
                  value: 15000000,
                  img_link: 'https://neal.fun/spend/minified/f16.jpg'
                },
            ],

            identities: [
                  { name: 'Bill Gates', 
                    networth: 104800000000, 
                    img_link: 'https://cdn.thegentlemansjournal.com/wp-content/uploads/2015/10/front18-900x600-c-center.jpg'
                  },
                  { name: 'Warren Buffett', 
                    networth: 66700000000, 
                    img_link: 'https://image.cnbcfm.com/api/v1/image/105000262-1568983881762gettyimages-50370540r.jpg'
                  },
                  { name: 'Jeff Bezos', 
                    networth: 143000000000, 
                    img_link: 'https://www.washingtonian.com/wp-content/uploads/2019/12/ajumpbasement10.SPREAD-994x731.jpg'
                  },
                  { name: 'Amancio Ortega', 
                    networth: 62700000000, 
                    img_link: 'http://www.european-leaders.com/wp-content/uploads/2018/07/Amancio-Ortega.jpg'
                  },
                  { name: 'Mark Zuckerberg', 
                    networth: 62300000000, 
                    img_link: 'https://image.cnbcfm.com/api/v1/image/105778243-1551893027826zuck19.jpg'
                  }
            ],

            seledtedItems: [],

        }
    },

    computed: {
        showBalance: function() {
            let subtotal = 0;

            this.seledtedItems.forEach(function(item) {
                subtotal += item.value*item.qnty;
            });

            return this.selectedIdentity.networth - subtotal;

        }
    },

    methods: {

        getSelectedIdentity: function(identity) {
            this.selectedIdentity = identity;
            this.is_identityNotEmpty = true;
        },

        formatMoney: function(amount) {
            return '$' + amount.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
        },

        calculate_items: function(data) {

            let itemIndex = this.seledtedItems.findIndex(function(currentItem) {
                return currentItem.id === data[0].id;
            });

            if(itemIndex >= 0) {
                if(data[1] === 'sell' && this.seledtedItems[itemIndex].qnty !== 0) {
                    return this.seledtedItems[itemIndex].qnty--;
                } 
                else {
                    return this.seledtedItems[itemIndex].qnty++;
                }
            }

            this.seledtedItems.push({
                ...data[0],
                qnty: 1
            });

        }


    }



}
</script>

<style>

    @import url("./assets/css/tailwind.css");


    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        /*margin-top: 60px;*/
    }

    input[type="number"]::-webkit-outer-spin-button, 

    input[type="number"]::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }

    input[type="number"] {
        -moz-appearance: textfield;
    }


</style>
