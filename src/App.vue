<template>
    <div>
        <!-- <my-drawer></my-drawer> -->
        <div class="bg-white w-4/5 mx-auto rounded-xl shadow-xl mt-14">
            <my-header></my-header>

            <div class="p-10">
                <div class="flex justify-between items-center">
                    <h2 class="text-3xl font-bold mb-8">Всі кросівки</h2>
                    <div class="flex gap-4">
                        <select class="py-2 px-3 border rounded outline-none">
                            <option>По назві</option>
                            <option>По ціні, дешевші</option>
                            <option>По ціні, дорожчі</option>
                        </select>

                        <div class="relative">
                            <img class="absolute left-4 top-3" src="/search.svg" alt="search" />
                            <input class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
                                type="text" placeholder="Пошук..." name="" value="" />
                        </div>
                    </div>
                </div>

                <my-card-list :items="items"></my-card-list>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

import MyCardList from "./components/MyCardList.vue";
import MyHeader from "./components/MyHeader.vue";

export default {
    components: { MyHeader, MyCardList },
    data() {
        return {
            items: []
        };
    },
    mounted() {
        axios.get('https://learning-autumn.github.io/Sneakers-JSON/Snealers.json')
            .then(resp => {
                this.items = resp.data;
            })
            .catch(error => console.error(error));
    }
};
</script>

<style lang="scss" scoped></style>
