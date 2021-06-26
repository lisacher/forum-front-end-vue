<template>
    <div class="card">
      <div class="card-header">
        <strong>{{ user.FavoritedRestaurants.length }}</strong> 收藏的餐廳
      </div>
      <div class="card-body">
        <router-link 
        :to="{ name: 'restaurant', params: { id: favoriteRestaurant.id } }"
        v-for="favoriteRestaurant in user.FavoritedRestaurants" :key="favoriteRestaurant.id"
        >
        <img
          :src="favoriteRestaurant.image"
          width="60"
          height="60"
          class="avatar"
        />
        </router-link>
      </div>
    </div>
</template>

<script>
export default {
  name: 'UserFavoritedRestaurantCard',
  props: {
    user: {
      type: Object,
      required: true
    }
  }
}
</script>