<template>
  <div class="container py-5">
    <AdminRestaurantForm 
      :initial-restaurant="restaurant"
      :is-processing="isProcessing" 
      @after-submit="submit"
    />
  </div>
</template>

<script>
import AdminRestaurantForm from '../components/AdminRestaurantForm.vue'
import adminAPI from './../apis/admin'
import { Toast } from './../utils/helpers'


export default {
  name: 'AdminRestaurantEdit',
  components: {
    AdminRestaurantForm
  },
  data() {
    return {
      restaurant: {
        id: -1,
        name: '',
        categoryId: '',
        tel: '',
        address: '',
        description: '',
        image: '',
        openingHours: ''
      },
      isProcessing: false
    }
  },
  methods: {
    async fetchData(restaurantId) {
      try {
      const { data } = await adminAPI.restaurants.getDetail({ restaurantId })
      const {
          id,
          name,
          tel,
          address,
          opening_hours: openingHours,
          description,
          image,
          CategoryId: categoryId
        } = data.restaurant
        
      this.restaurant = {
        ...this.restaurant,
        id,
        name,
        categoryId,
        tel,
        address,
        description,
        image,
        openingHours
      }
      }
      catch (error) {
        this.isProcessing = false
        Toast.fire({
          icon: 'error',
          title: '無法取得餐廳資料，請稍後再試'
        })
      }
    },
    async submit(formData) {
      try {
        this.isProcessing = true
        const { data } = await adminAPI.restaurants.update({ restaurantId: this.restaurant.id, formData})
        
        if (data.status !== 'success') {
          throw new Error(data.message)
        }
        
        this.$router.push({ name: 'admin-restaurants' })
      }
      catch (error) {
        this.isProcessing = false
        Toast.fire({ 
          icon: 'error',
          title: '無法送出資料，請稍後再試'
        })
      }
    }
  },
  created() {
    const { id } = this.$route.params
    this.fetchData(id)
  },
  beforeRouteUpdate (to, from, next) {
    // 路由改變時重新抓取資料
    const { id } = to.params
    this.fetchData(id)
    next()
  }
}
</script>
