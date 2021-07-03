<template>
  <div class="container py-5">
     <!-- 使用先前寫好的 AdminNav -->
    <AdminNav />

    <form class="my-4">
      <div class="form-row">
        <div class="col-auto">
          <input
            type="text"
            v-model="newCategoryName"
            class="form-control"
            placeholder="新增餐廳類別..."
          >
        </div>
        <div class="col-auto">
          <button
            type="button"
            @click.prevent.stop="createCategory"
            :is-processing="isProcessing"
            class="btn btn-primary"
          >
            新增
          </button>
        </div>
      </div>
    </form>
    <table class="table">
      <thead class="thead-dark">
        <tr>
          <th
            scope="col"
            width="60"
          >
            #
          </th>
          <th scope="col">
            Category Name
          </th>
          <th
            scope="col"
            width="210"
          >
            Action
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
      v-for="category in categories"
      :key="category.id"
    >
     <th scope="row">
      {{ category.id }}
     </th>
        <td class="position-relative">
          <div
            v-show="!category.isEditing"
            class="category-name"
          >
            {{ category.name }}
          </div>
          <input
            v-show="category.isEditing"
            v-model="category.name"
            type="text"
            class="form-control"
          >
          <span
            v-show="category.isEditing"
            @click="handleCancel(category.id)"
            class="cancel"
          >
            ✕
          </span>
        </td>
        <td class="d-flex justify-content-between">
          <button
            v-show="!category.isEditing"
            @click.prevent.stop="toggleIsEditing(category.id)"
            type="button"
            class="btn btn-link mr-2"
          >
            Edit
          </button>
          <button
            v-show="category.isEditing"
            type="button"
            @click.stop.prevent="
            updateCategory({ categoryId: category.id, name: category.name })"
            class="btn btn-link mr-2"
          >
            Save
          </button>
          <button
            type="button"
            class="btn btn-link mr-2"
            @click.stop.prevent="deleteCategory(category.id)"
          >
            Delete
          </button>
        </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
/* eslint-disable */
import {v4 as uuidv4} from 'uuid'
import AdminNav from '@/components/AdminNav'
import adminAPI from './../apis/admin'
import { Toast } from './../utils/helpers'



export default {
  name: 'AdminCategories',
  components: {
    AdminNav
  },
  // 定義 Vue 中使用的 data 資料
  data () {
    return {
      categories: [],
      newCategoryName : '',
      isProcessing: false
    }
  },
  // 調用 `fetchCategories` 方法
  created () {
    this.fetchCategories()
  },
  methods: {
    //  定義 `fetchCategories` 方法
    async fetchCategories () {
      try {
        const { data } = await adminAPI.categories.get()
        this.categories = data.categories.map(category => ({
          ...category,
          isEditing: false,
          nameCached: ''
        }))

        if (data.status == 'error') {
          throw new Error(data.message)
        }

      }
      catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法顯示餐廳類別，請稍後再試'
        })
      }
    },
    async createCategory () {
      try {
        this.isProcessing = true
        const { data } = await adminAPI.categories.create({ name: this.newCategoryName })     
      
        if (data.status === 'error') {
            throw new Error(data.message)
        }

        // 將新的類別添加到陣列中
        this.categories.push({
          isEditing: false,
          id: data.categoryId,
          name: this.newCategoryName
        })
        this.isProcessing = false
        this.newCategoryName = '' // 清空原本欄位中的內容
      }
      catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法新增餐廳類別，請稍後再試'
        })
      }
    },
    async deleteCategory (categoryId) {
      try {
        const { data } = await adminAPI.categories.delete({ categoryId })

        if (data.status === 'error') {
            throw new Error(data.message)
        }

        this.categories = this.categories.filter(
          category => category.id !== categoryId
        )

        Toast.fire({
          icon: 'success',
          title: '您已刪除該餐廳類別'
        })
      }
      catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法刪除廳類別，請稍後再試'
        })
      }
    },
    toggleIsEditing (categoryId) {
    this.categories = this.categories.map(category => {
        if (category.id === categoryId) {
        return {
            ...category,
            isEditing: !category.isEditing,
            nameCached: category.name
        }
        }

        return category
    })
    },
    async updateCategory ({ categoryId, name }) {
      try {
        const { data } = await adminAPI.categories.update({ categoryId, name })
        this.toggleIsEditing(categoryId)

        if (data.status === 'error') {
          throw new Error(data.message)
        }
      }
      catch (error) {
        Toast.fire({
          icon: 'error',
          title: '無法編輯餐廳類別，請稍後再試'
        })
      }
    },
    handleCancel (categoryId) {
    this.categories = this.categories.map(category => {
        if (category.id === categoryId) {
        return {
            ...category,

            // 把原本的餐廳類別名稱還回去
            name: category.nameCached
        }
    }
    return category
    })
    this.toggleIsEditing(categoryId)
    }
  }
}
</script>

<style scoped>
.category-name {
  padding: 0.375rem 0.75rem;
  border: 1px solid transparent;
  outline: 0;
  cursor: auto;
}

.btn-link {
  width: 62px;
}

.cancel {
  position: absolute;
  right: 20px;
  top: 50%;
  transform: translateY(-50%);
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 25px;
  height: 25px;
  border: 1px solid #aaaaaa;
  border-radius: 50%;
  user-select: none;
  cursor: pointer;
  font-size: 12px;
}
</style>