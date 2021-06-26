<template>
  <div id="restaurant-form">
    <Navbar />
    <main
      role="main"
      class="mt-5 bg-white"
    >
      <router-view />
    </main>
    
  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.min.css'
import 'bootstrap'
import Navbar from './components/Navbar'

export default {
  components: {
    Navbar
  }
}
</script>
