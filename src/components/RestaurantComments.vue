<template>
  <div>
    <h2 class="my-4">
      所有評論：
    </h2>

    <div v-for="comment in restaurantComments" :key="comment.id">
      <blockquote class="blockquote mb-0">
        <button
          type="button"
          class="btn btn-danger float-right"
           v-if="currentUser.isAdmin"
           @click.stop.prevent="handleDeleteButtonClick(comment.id)"
        >
          Delete
        </button>
        <h3>
          <router-link :to="{ name: 'users', params: { id: comment.User.id } }">
            {{ comment.User.name }}
          </router-link>
        </h3>
        <p>{{ comment.text }}</p>
        <footer class="blockquote-footer">
          {{ comment.createdAt | fromNow }}
        </footer>
      </blockquote>
      <hr>
    </div>
  </div>
</template>

<script>
import { fromNowFilter } from './../utils/mixins'
import commentsAPI from './../apis/comments'
import { Toast } from './../utils/helpers'
import { mapState } from 'vuex'

export default {
    mixins: [fromNowFilter],
  props: {
    restaurantComments: {
      type: Array,
      required: true
    }
  },
  computed: {
    ...mapState(['currentUser'])
  },
  methods: {
    async handleDeleteButtonClick (commentId) {
      try{
        const { data } = await commentsAPI.delete({ commentId })
        // 觸發父層事件 - $emit( '事件名稱' , 傳遞的資料 )
        this.$emit('after-delete-comment', commentId)

        Toast.fire({
          icon: 'success',
          title: '移除評論成功'
        })

        if (data.status === 'error') {
            throw new Error(data.message)
        }
      }
      catch (error){
        Toast.fire({
          icon:'error',
          title:'無法移除評論資料，請稍後再試'
        })
      }
    }
  }
}
</script>