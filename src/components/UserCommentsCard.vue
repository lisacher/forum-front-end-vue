<template>
  <div class="card">
    <div class="card-header"><strong>{{ user.Comments.length }}</strong> 已評論餐廳</div>
    <div class="card-body">
      <router-link 
        :to="{ name: 'restaurant', params: { id: comment.id } }" 
        v-for="comment in user.Comments" 
        :key="comment.id"
      >
        <img :src="comment.Restaurant.image" width="60" height="60" class="avatar">
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserCommentsCard',
  props: {
    user: {
      type: Object,
      required: true
    }
  }
}
</script>