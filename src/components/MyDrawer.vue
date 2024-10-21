<template>
    <div class="">
        <div @click="closeDrawer" class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
        <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
            <my-drawer-head></my-drawer-head>
            <my-card-item-list></my-card-item-list>

            <div class="flex flex-col gap-4 mt-7">
                <div class="flex gap-2">
                    <span>Всього:</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{ totalPrice }} ₴</b>
                </div>
                <div class="flex gap-2">
                    <span>Податок 5%:</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{ vatPrice }} ₴</b>
                </div>
                <button :disabled="totalPrice ? false : true"  @click="handleCreateOrder"
                    class="mt-4 transition bg-lime-500 w-full rounded-xl py-4 text-white disable:bg-slate-400 hover:bg-lime-600 active:bg-lime-700 cursor-pointer">
                    Оформити замовлення
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import MyCardItemList from "./MyCardItemList.vue";
import MyDrawerHead from "./MyDrawerHead.vue";
export default {
    components: { MyDrawerHead, MyCardItemList },
    props: {
        closeDrawer: {
            type: Function,
            required: true
        },
        totalPrice: Number,
        vatPrice: Number,
    },
    methods: {
        handleCreateOrder() {
            this.$emit('create-order');
        }
    }
};
</script>

<style lang="scss" scoped></style>
