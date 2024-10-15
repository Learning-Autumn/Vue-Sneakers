<template>
    <div>
        <div class="grid grid-cols-4 gap-5">
            <my-card 
            title="Мужские крассовки Nike Blazer Mid Suede" 
            :price="5000" 
            image-url="/sneakers/sneakers-1.jpg" 
            :is-added="true"
            :is-favorite="true"
            :onClickAdd="onClickAdd"
            ></my-card>
        </div>
    </div>
</template>

<script>
import MyCard from './MyCard.vue';

    export default {
  components: { MyCard },
    methods: {
        onClickAdd() {
            alert(111)
        }
    },   
    }
</script>

<style lang="scss" scoped>

</style>