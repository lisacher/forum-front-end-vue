<template>
    <div class="card">
      <div class="card-header">
        <strong>{{ user.Followings.length }}</strong> followings (追蹤者)
      </div>
      <div class="card-body">
        <router-link 
        :to="{ name: 'user', params: { id: following.id } }"
        v-for="following in user.Followings" 
        :key="following.id"
        >
        <img :src="following.image" width="60" height="60" class="avatar">
      </router-link>
      </div>
    </div>
</template>

<script>
export default {
  name: 'UserFollowingsCard',
  props: {
    user: {
      type: Object,
      required: true
    }
  }
}
</script>