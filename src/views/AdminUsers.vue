<template>
  <div class="container py-5">
    <!-- AdminNav Component -->
    <AdminNav />
    <table class="table">
      <thead class="thead-dark">
        <tr>
          <th scope="col">
            #
          </th>
          <th scope="col">
            Email
          </th>
          <th scope="col">
            Role
          </th>
          <th
            scope="col"
            width="140"
          >
            Action
          </th>
        </tr>
      </thead>
      <tbody>
        <tr 
        v-for="user in users"
        :key="user.id"
        >
          <th scope="row">
            {{ user.id }}
          </th>
          <td>{{ user.email }}</td>
          <td>{{ user.isAdmin ? 'Admin':'User'  }}</td>
          <td>
            <button
              type="button"
              class="btn btn-link"
              v-if="currentUser.id !== user.id"
            @click.prevent.stop="toggleUserRole(user.id, user.isAdmin)"
            >
              {{ user.isAdmin ? 'set as user' : 'set as admin' }}
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import AdminNav from '@/components/AdminNav'
import { mapState } from 'vuex'
import adminAPI from './../apis/admin'
import { Toast } from './../utils/helpers'


export default {
  name: 'AdminUsers',
  components : {
      AdminNav
  },
  data () {
  return {
    users: []
  }
  },
  computed: {
    ...mapState(['currentUser'])
  },
  created() {
      this.fetchUsers()
  },
  methods: {
      async fetchUsers() {
      try {
        const { data } = await adminAPI.users.get()
        console.log('data: ', data)
        this.users = data.users
      }
      catch (error) {
        console.log('error: ', error)
        Toast.fire({
          icon: 'error',
          title: '無法顯示使用者清單，請稍後再試',
        })
      }
    },
      async toggleUserRole(userId, isAdmin) {
      try {
        const { data } = await adminAPI.users.update({
          userId,
          isAdmin: (!isAdmin).toString(),
        })
        if (data.status !== 'success') {
          throw new Error(data.message);
        }
        Toast.fire({
          icon:'success',
          title:'更新完成'
        })

        this.users = this.users.map((user) => {
          if (user.id === userId) {
            return {
              ...user,
              isAdmin: !user.isAdmin,
            }
          }
          return user
        })
      } 
      
      catch (error) {
        console.log('error: ', error)
        Toast.fire({
          icon: 'error',
          title: '目前無法更新使用者權限，請稍後再試',
        })
      }
    }
  }
}
</script>