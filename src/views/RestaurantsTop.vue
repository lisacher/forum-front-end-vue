<template>
  <div class="container py-5">
    <NavTabs />
    <h1 class="mt-5">
      人氣餐廳
    </h1>

    <hr>
    <div
      class="card mb-3"
      style="max-width: 540px;margin: auto;"
    >
      <div class="row no-gutters"
      v-for="restaurant in restaurants"
      :key="restaurant.id">
        <div class="col-md-4">
          <router-link 
          :to="{ name: 'restaurant', params: { id: restaurant.id }}">
            <img
              class="card-img"
              :src="restaurant.image"
            >
          </router-link>
        </div>
        <div class="col-md-8">
          <div class="card-body">
            <h5 class="card-title">
              {{ restaurant.name }}
            </h5>
            <span class="badge badge-secondary">收藏數：{{restaurant.FavoriteCount }}</span>
            <p class="card-text">
              {{ restaurant.description }}
            </p>
            <router-link
              :to="{ name: 'restaurant', params: { id: restaurant.id }}"
              class="btn btn-primary mr-2"
            >Show</router-link>

            <button
              type="button"
              class="btn btn-danger mr-2"
              v-if="restaurant.isFavorited"
              @click.stop.prevent="deleteFavorite(restaurant.id)"
            >
              移除最愛
            </button>
            <button
              type="button"
              class="btn btn-primary"
              v-else
              @click.stop.prevent="addFavorite(restaurant.id)"
            >
              加到最愛
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import NavTabs from './../components/NavTabs.vue'
import restaurantsAPI from './../apis/restaurants'
import { Toast } from './../utils/helpers'
import usersAPI from './../apis/users'

export default {
  components: {
    NavTabs
  },
  data() {
    return {
      restaurants: []
    }
  },
  created() {
    this.fetchRestaurants()
  },
  methods: {
    async fetchRestaurants() {
      try {
        const { data } = await restaurantsAPI.getTopRestaurants()
        if (data.status === 'error') {
          throw new Error(data.message)
        }
        this.restaurants = data.restaurants
      } 
      
      catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法取得餐廳資料，請稍後再試'
        })
      }
    
    
    },
    async deleteFavorite(restaurantId) {
      try {
        const { data } = await usersAPI.deleteFavorite({ restaurantId })

        if (data.status !== 'success') {
              throw new Error(data.message)
        }

        this.restaurants = this.restaurants
          .map(restaurant => {
            if (restaurant.id !== restaurantId) {
              return restaurant
            }
            return {
              ...restaurant,
              FavoriteCount: restaurant.FavoriteCount - 1,
              isFavorited: false
            }
          })
      } 
      catch(error) {
        Toast.fire({
            icon: 'error',
            title: '無法將餐廳移除最愛，請稍後再試'
        })
      }
    },
    async addFavorite(restaurantId) {
      try {
        const { data } = await usersAPI.addFavorite({ restaurantId })

        if (data.status !== 'success') {
              throw new Error(data.message)
        }

        this.restaurants = this.restaurants
          .map(restaurant => {
            if (restaurant.id !== restaurantId) {
              return restaurant
            }
            return {
              ...restaurant,
              FavoriteCount: restaurant.FavoriteCount + 1,
              isFavorited: true
            }
          })
      } 
      catch(error) {
        Toast.fire({
            icon: 'error',
            title: '無法將餐廳加入最愛，請稍後再試'
        })
      }
    }
  }
}
</script>