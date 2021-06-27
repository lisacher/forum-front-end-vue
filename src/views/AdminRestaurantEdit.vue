<template>
  <div class="container py-5">
    <AdminRestaurantForm 
      :initial-restaurant="restaurant" 
      @after-submit="submit"
    />
  </div>
</template>

<script>
import AdminRestaurantForm from '../components/AdminRestaurantForm.vue'
const dummyData = {
  'restaurant': {
    'id': 1,
    'name': 'Laurence Reynolds',
    'tel': '1-657-067-3756 x9782',
    'address': '187 Kirlin Squares',
    'opening_hours': '08:00',
    'description': 'sit est mollitia',
    'image': 'https://loremflickr.com/320/240/restaurant,food/?random=91.29816290184887',
    'CategoryId': 3,
  }
}
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
      }
    }
  },
  methods: {
    fetchData() {
      const { restaurant } = dummyData
      const { id, name, CategoryId: categoryId, tel, address, description, image, opening_hours: openingHours } = restaurant
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
    },
    submit(formData) {
      // 將表單資料透過 API 傳給後端
      for (let [name, value] of formData.entries()) {
        console.log(`${name} : ${value}`)
      }
    }
  },
  created() {
    this.fetchData()
  }
}
</script>
