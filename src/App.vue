<template>
    <div class="bg-white w-4/5 mx-auto  rounded-xl shadow-xl mt-14">

        <my-header></my-header>

        <div class="p-10">
            <h2 class="text-3xl font-bold mb-8">Всі кросівки</h2>

            <my-card-list></my-card-list>
        </div>

    </div>
</template>

<script>
import MyCardList from './components/MyCardList.vue';
import MyHeader from './components/MyHeader.vue';

export default {
    components: { MyHeader, MyCardList },};
</script>

<style lang="scss" scoped></style>
