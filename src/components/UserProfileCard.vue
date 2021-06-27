<template>
  <div class="card mb-3">
    <div class="row no-gutters">
      <div class="col-md-4">
        <img
          :src="user.image"
          width="300px"
          height="300px"
        />
      </div>
      <div class="col-md-8">
        <div class="card-body">
          <h5 class="card-title">{{ user.name }}</h5>
          <p class="card-text">
            {{ user.email }}
          </p>
          <ul class="list-unstyled list-inline">
            <li><strong>{{ user.Comments.length }}</strong> 已評論餐廳</li>
            <li><strong>{{ user.FavoritedRestaurants.length }}</strong> 收藏的餐廳</li>
            <li><strong>{{ user.Followings.length }}</strong> followings (追蹤者)</li>
            <li><strong>{{ user.Followers.length }}</strong> followers (追隨者)</li>
          </ul>
          <p>
            <router-link 
              :to="{ name: 'user-edit', params: { id: user.id } }"
              v-if="user.isAdmin"
            >
              <button 
                type="submit" 
                class="btn btn-primary"
              >edit</button>
            </router-link>
            <template v-else>
              <button 
                type="button" 
                class="btn btn-primary"
                @click.prevent.stop="handleFollow"
                v-if="!isFollowed"
              >追蹤</button>
              <button 
                type="button" 
                class="btn btn-danger"
                @click.prevent.stop="handleUnfollow"
                v-else
              >取消追蹤</button>
            </template>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { emptyImageFilter } from './../utils/mixins'

export default {
  mixins: [emptyImageFilter],
  name: 'UserProfileCard',
  props: {
    user: {
      type: Object,
      required: true
    }
  },
  methods: {
    handleFollow() {
      this.$emit('follow')
    },
    handleUnfollow() {
      this.$emit('unfollow')
    },
  }
}
</script>
