<template>
    <div>
        <my-drawer v-if="drawerOpen" 
        :closeDrawer="closeDrawer" 
        :total-price="totalPrice" 
        :vatPrice="vatPrice"
        @create-order="createOrder"
        :isCreatingOrder="isCreatingOrder"></my-drawer>

        <div class="bg-white w-4/5 mx-auto rounded-xl shadow-xl mt-14">
            <my-header :total-price="totalPrice" :openDrawer="openDrawer"></my-header>

            <div class="p-10">
                <div class="flex justify-between items-center">
                    <h2 class="text-3xl font-bold mb-8">Всі кросівки</h2>
                    <div class="flex gap-4">
                        <select @change="onChangeSelect" class="py-2 px-3 border rounded outline-none">
                            <option value="title">По назві</option>
                            <option value="price">По ціні, дешевші</option>
                            <option value="-price">По ціні, дорожчі</option>
                        </select>

                        <div class="relative">
                            <img class="absolute left-4 top-3" src="/search.svg" alt="search" />
                            <input @input="onChangeInput" v-model="filters.searchQuery"
                                class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400" type="text"
                                placeholder="Пошук..." />
                        </div>
                    </div>
                </div>

                <my-card-list :items="items"></my-card-list>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import MyCardList from "./components/MyCardList.vue";
import MyHeader from "./components/MyHeader.vue";
import MyDrawer from "./components/MyDrawer.vue";

export default {
    components: { MyHeader, MyCardList, MyDrawer },
    provide() {
        return {
            AddToFavorite: this.AddToFavorite,
            addToCart: this.addToCart,
            removeFromCart: this.removeFromCart,
            closeDrawer: this.closeDrawer,
            cart: this.cart,
        };
    },
    data() {
        return {
            items: [],
            favorites: [],
            cart: [],
            drawerOpen: false,
            isCreatingOrder: false,
            filters: {
                sortBy: "title",
                searchQuery: "",
            },
        };
    },
    computed: {
        totalPrice() {
            return this.cart.reduce((acc, item) => acc + item.price, 0);
        },
        vatPrice() {
            return this.totalPrice * 0.05;
        },
    },
    methods: {
        async fetchItems() {
            try {
                const { data } = await axios.get(`https://e497329b2c6762bd.mokky.dev/items`, {
                    params: {
                        title: `*${this.filters.searchQuery}*`,
                        sortBy: this.filters.sortBy,
                    },
                });
                this.items = data.map((obj) => ({
                    ...obj,
                    isFavorite: false,
                    isAdded: false,
                }));
            } catch (err) {
                console.error(err);
            }
        },
        async fetchFavorites() {
            try {
                const { data: favoriteItems } = await axios.get(
                    `https://e497329b2c6762bd.mokky.dev/favorites`
                );
                this.items = this.items.map((item) => {
                    const favorite = favoriteItems.find(
                        (favorite) => favorite.parentId === item.id
                    );

                    if (!favorite) {
                        return item;
                    }

                    return {
                        ...item,
                        isFavorite: true,
                        favoriteId: favorite.id,
                    };
                });
            } catch (err) {
                console.error(err);
            }
        },
        async AddToFavorite(item) {
            try {
                const favorite = this.favorites.find((fav) => fav.parentId === item.id);

                if (favorite) {
                    item.isFavorite = false;
                    await axios.delete(
                        `https://e497329b2c6762bd.mokky.dev/favorites/${favorite.id}`
                    );
                    this.favorites = this.favorites.filter((fav) => fav.parentId !== item.id);
                } else {
                    item.isFavorite = true;
                    const { data } = await axios.post(
                        `https://e497329b2c6762bd.mokky.dev/favorites`,
                        {
                            parentId: item.id,
                        }
                    );
                    item.favoriteId = data.id;
                    this.favorites.push({ parentId: item.id, id: data.id });
                }
            } catch (err) {
                console.error("Не вдалося змінити статус улюбленого товару", err);
            }
        },
        async addToCart(item) {
            const cartItem = this.cart.find((cartItem) => cartItem.id === item.id);

            if (cartItem) {
                item.isAdded = false;
                this.cart = this.cart.filter((cartItem) => cartItem.id !== item.id);
            } else {
                item.isAdded = true;
                this.cart.push(item); 
            }
            const originalItem = this.items.find((original) => original.id === item.id);
            if (originalItem) {
                originalItem.isAdded = item.isAdded;
            }

            
        },
        async removeFromCart(itemToRemove) {
            const index = this.cart.findIndex(item => item.id === itemToRemove.id);
            if (index !== -1) {
                this.cart.splice(index, 1);

                const item = this.items.find(item => item.id === itemToRemove.id);
                if (item) {
                    item.isAdded = false;
                }
            }
        },
        async closeDrawer() {
            this.drawerOpen = false;
        },
        async openDrawer() {
            this.drawerOpen = true;
        },
        async createOrder() {
            try {
                this.isCreatingOrder = true
                this.items.forEach(item => {
                    item.isAdded = false;
                });
                this.cart.forEach(item_cart => {
                    item_cart.isAdded = false;
                });
                


                const { data } = await axios.post('https://e497329b2c6762bd.mokky.dev/orders', {
                    items: this.cart,
                    totalPrice: this.totalPrice,
                });

                this.cart = [];
                

                return data;
            } catch (err) {
                console.log(err);
            } finally {
              this.isCreatingOrder = false
            }
        },
        onChangeSelect(event) {
            this.filters.sortBy = event.target.value;
            if (this.filters.sortBy !== "Сортування") {
                this.fetchItems();
            }
        },
        onChangeInput() {
            this.fetchItems();
        },
    },
    mounted() {
        this.fetchItems();
        this.fetchFavorites();
    },
    watch: {
        filters: {
            deep: true,
            handler() {
                this.fetchItems();
            },
        },
    },
};
</script>

<style scoped></style>
