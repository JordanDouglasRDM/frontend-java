<script>
import ContainerBox from "../components/ContainerBox.vue";
import Navbar from "../components/Navbar.vue";
import Swal from "sweetalert2";

export default {
  name: "Register",
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
    async store() {
      try {
        const form = new FormData(document.getElementById("form-store"));
        const newUser = {
          username: form.get("username"),
          email: form.get("email"),
          password: form.get("password"),
          cep: form.get("cep"),
        }

        console.log(newUser)
        // const response = await axios.post("/register", credenptials);
        // if (response.data.status !== 201) {
        //   throw new Error('Houve um erro ao cadastrar um novo usuário. Entre em contato com o adm do sistema.')
        // }
        this.toast.fire({
          icon: "success",
          title: "Usuário cadastrado com sucesso",
          timer: 2000
        });
      } catch (error) {
        await this.toast.fire({
          icon: "error",
          title: "Falha ao cadastrar",
          text: error.message
        });
      }
    }
  }
}
</script>

<template>
  <Navbar/>
  <ContainerBox>
    <h2 class="text text-center">Registre-se</h2>
    <form id="form-store" class="w-100 d-flex flex-column gap-4">
      <div class="">
        <label for="username" class="form-label">Nome:</label>
        <input type="email" class="form-control" id="username" name="username" autofocus required>
      </div>
      <div class="mb-4">
        <label for="email" class="form-label">Email:</label>
        <input type="email" class="form-control" id="email" name="email" required>
      </div>
      <div class="mb-4">
        <label for="password" class="form-label">Senha:</label>
        <input type="password" class="form-control" id="password" name="password" required>
      </div>
      <div class="mb-4">
        <label for="cep" class="form-label">CEP:</label>
        <input type="tel" class="form-control" id="cep" name="cep" required>
        <router-link class="link" to="/">Voltar para login</router-link>
      </div>
      <div class="d-flex justify-content-between">
        <button @click="store" type="button" class="btn btn-success w-100">Registrar</button>
      </div>
    </form>
  </ContainerBox>
</template>

<style scoped>

</style>