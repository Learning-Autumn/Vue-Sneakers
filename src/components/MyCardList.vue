<template>
  <div>
    <div class="grid grid-cols-4 gap-5">
      <my-card
        v-for="item in items"
        :title="item.title"
        :price="item.price"
        :image-url="item.imageUrl"
        :is-added="true"
        :is-favorite="true"
        @clickAdd="onClickAdd(item)"
        @clickFavorite="onClickFavorite(item)"
        :key="item.id"
      ></my-card>
    </div>
  </div>
</template>

<script>
import MyCard from "./MyCard.vue";

export default {
  components: { MyCard },
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
  methods: {
    onClickAdd() {
      alert("Added to cart!");
    },
    onClickFavorite() {
      alert("Toggled favorite!");
    },
  },
};
</script>

<style lang="scss" scoped></style>
