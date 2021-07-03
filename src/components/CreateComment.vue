<template>
  <form @submit.stop.prevent="handleSubmit">
    <div class="form-group mb-4">
      <label for="text">留下評論：</label>
      <textarea
      v-model="text"
        class="form-control"
        rows="3"
        name="text"
      />
    </div>
    <div class="d-flex align-items-center justify-content-between">
      <button
        type="button"
        class="btn btn-link"
        @click="$router.back()"
      >回上一頁</button>
      <button
        type="submit"
        :is-processing="isProcessing"
        class="btn btn-primary mr-0"
      >
        Submit
      </button>
    </div>
  </form>
</template>

<script>

import commentsAPI from './../apis/comments'
import { Toast } from '../utils/helpers'

export default {
  props: {
    restaurantId: {
      type: Number,
      required: true
    }
  },
  data () {
    return {
      text: '',
      isProcessing: false
    }
  },
  methods: {
    async handleSubmit () {
      try {
        this.isProcessing= true
        const { data } = await commentsAPI.create({restaurantId: this.restaurantId, text: this.text})
          
          if (!this.text) {
          Toast.fire({
            icon: 'warning',
            title: '請填寫評論內容'
          })
          return
        }

          if (data.status === 'error') {
            throw new Error(data.message)
          }

        this.$emit('after-create-comment', {
          commentId: data.commentId,
          restaurantId: this.restaurantId,
          text: this.text
        })
        Toast.fire ({
          icon: 'success',
          title: '評論成功！'
        })
        this.isProcessing = false
        this.text = '' // 將表單內的資料清空
      }
      catch (error) {
        console.error(error.message)
        Toast.fire ({
          icon: 'error',
          title: '無法新增評論，請稍後再試'
        })
      }
    }
  }
}
</script>