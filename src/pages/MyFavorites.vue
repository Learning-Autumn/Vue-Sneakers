<template>
  <div>
    <h1>Мої Закладки</h1>
    <my-card-list :items="favorites"></my-card-list>
  </div>
</template>

<script>
import MyCardList from '@/components/MyCardList.vue';
import axios from 'axios';

export default {
  components: { MyCardList },
  data() {
    return {
      favorites: []
    };
  },
  created() {
    this.fetchFavorites();
  },
  methods: {
    async fetchFavorites() {
      try {
        const { data } = await axios.get('https://e497329b2c6762bd.mokky.dev/favorites');
        // console.log(data);
        this.favorites = data.filter(obj => obj.item.isFavorite).map(obj => obj.item);
      } catch (err) {
        console.log(err);
      }
    }
  }
};
</script>

<style lang="scss" scoped></style>
