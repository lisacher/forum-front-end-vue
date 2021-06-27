<template>
 <div class="container py-5">
    <!-- 餐廳表單 AdminRestaurantForm -->
    <AdminRestaurantForm 
      @after-submit="submit"
    />
  </div>
</template>

<script>
import AdminRestaurantForm from './../components/AdminRestaurantForm.vue'
export default {
  name: 'AdminRestaurantNew',
  components: {
    AdminRestaurantForm
  },
  methods: {
    submit(formData) {
      // 將表單資料透過 API 傳給後端
      for (let [name, value] of formData.entries()) {
        console.log(`${name} : ${value}`)
      }
    }
  }
}
</script>