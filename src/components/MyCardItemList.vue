<template>
    <div class="flex flex-col flex-1 gap-4 justify-between">
        <my-card-item v-for="item in cart" :key="item.id" :title="item.title" :id="item.id" :price="item.price" :imageUrl="item.imageUrl"></my-card-item>
        <p v-if="cart.length === 0">Корзина порожня</p>
    </div>
</template>

<script>
import MyCardItem from "./MyCardItem.vue";
export default {
    inject: ['cart', 'closeDrawer', 'addToCart'],
    components: { MyCardItem },
};
</script>

<style lang="scss" scoped></style>
