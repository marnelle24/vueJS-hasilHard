<template>
    <div class="flex items-center bg-gray-200">
        <div v-for="identity in identities" :key="identity.name" class="flex-1 bg-gray-400 px-4 py-2 m-2 clearfix:after customDiv cursor-pointer" @click="setIdentity(identity)">
            <img :src="identity.img_link" alt="..." class="sm:float-left shadow-lg rounded-full max-w-full h-auto align-middle border-none cursor-pointer" />
            <h1 class="md:text-2xl xl:text-black font-extrabold cursor-pointer">{{ formatMoney(identity.networth) }}</h1>
            <h1 class="md:text-2xl xl:text-black font-extrabold cursor-pointer">{{ identity.name }}</h1>
        </div>
    </div>
</template>

<script>
export default {
    name: 'YourIdentity',
    props: ['identity_data'],
    data() {
        return {
            identities: []
        }
    },

    mounted() {
        this.identities = this.identity_data;
    },

    methods: {
        setIdentity: function(data) {
            this.$emit('getIdentity', data);
        },

        formatMoney: function(amount) {
            return '$' + amount.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

    .customDiv:hover {
        background: #e2e8f0;
    }
</style>
