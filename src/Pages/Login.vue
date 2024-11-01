<template>
  <ContainerBox>
    <h2 class="text text-center">Autenticação com Java e JWT</h2>
    <form id="form-login" class="w-100 d-flex flex-column gap-4" method="post" action="/">
      <div class="">
        <label for="email" class="form-label">Email:</label>
        <input type="email" class="form-control" id="email" name="email" autofocus required>
      </div>
      <div class="mb-4">
        <label for="password" class="form-label">Senha:</label>
        <input type="password" class="form-control" id="password" name="password" required>
        <router-link class="link" to="/register">Não possui cadastro? Registre-se agora.</router-link>
      </div>
      <div class="d-flex justify-content-between">
        <button type="submit" @click.prevent="login" class="btn btn-primary w-100">Login</button>
      </div>
    </form>
  </ContainerBox>
</template>

<style scoped>

</style>

<script>
import ContainerBox from "../components/ContainerBox.vue";
import Navbar from "../components/Navbar.vue";
import {axios} from "../main.js";
import Swal from "sweetalert2";

export default {
  name: "Login",
  components: {Navbar, ContainerBox},
  data() {
    return {
      toast: Swal.mixin({
        toast: true,
        position: "top-end",
        showConfirmButton: false,
        timer: 3000,
        timerProgressBar: true,
        didOpen: (toast) => {
          toast.onmouseenter = Swal.stopTimer;
          toast.onmouseleave = Swal.resumeTimer;
        }
      }),
    }
  },
  methods: {
    async login() {
      try {
        const form = new FormData(document.getElementById("form-login"));
        const credenptials = {
          email: form.get("email"),
          password: form.get("password"),
        }

        //const response = await axios.post("/user/auth", credenptials);
        //let value = response.data.token;
        let value = '6a54sd654asd456as-aqsd165as1d-ads1a45sd-asdasdasd';
        localStorage.setItem('token', value);
        this.toast.fire({
          icon: "success",
          title: "Login realizado com sucesso",
          timer: 2000
        });
        await this.canAccess()
      } catch (error) {
        await this.toast.fire({
          icon: "error",
          title: "Falha ao autenticar",
          text: error.message
        });
      }
    },
    async canAccess() {
      if (localStorage.getItem("token")) {
        this.$router.push('/users');
      }
      await this.toast.fire({
        icon: "error",
        title: "Usuário não autenticado",
        text: "Tente novamente mais tarde.",
        timer: 2500
      });
    }
  }
}
</script>