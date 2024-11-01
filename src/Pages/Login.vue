<template>
  <Navbar />
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
  mounted() {
    this.logout()
  },
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

        console.log(credenptials)
        localStorage.setItem('token', '321asd6a5s4dasd1asd54asd-asd16as5d4asd-1dasd46asd-13216a4dav');
        // const response = await axios.post("/login", credenptials);
        this.toast.fire({
          icon: "success",
          title: "Login realizado com sucesso",
          timer: 2000
        });
        return response.data;
      } catch (error) {
        await this.toast.fire({
          icon: "error",
          title: "Falha ao autenticar",
          text: error.message
        });
      }
    }, logout() {
      localStorage.removeItem('token');
    }
  }
}
</script>