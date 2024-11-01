<script>
export default {
  name: "Navbar",
  methods: {
    logout() {
      localStorage.removeItem('token');
      this.$router.push('/');
    }
  }
}
</script>

<template>
  <nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container-fluid">
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
          <router-link class="nav-link" :class="[$route.name === 'login' ? 'active' : '']" aria-current="page" to="/">Página de login</router-link>
          <router-link class="nav-link" :class="[$route.name === 'register' ? 'active' : '']" aria-current="page" to="/register">Registrar Novo</router-link>
          <router-link class="nav-link" :class="[$route.name === 'users' ? 'active' : '']" aria-current="page" to="/users">Listar Usuários</router-link>
        </div>
      </div>
    </div>
    <button class="btn btn-outline-danger" @click="logout">Sair</button>
  </nav>
</template>

<style scoped>

</style>