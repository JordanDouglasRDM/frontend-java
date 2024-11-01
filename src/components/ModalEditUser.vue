<script>
import axios from "axios";
import Swal from "sweetalert2";

export default {
  name: "ModalEditUser",
  props: {
    user: {
      type: Object,
      required: true
    }
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
    async edit() {
      try {
        const form = new FormData(document.getElementById("form-edit"));
        const credenptials = {
          email: form.get("email"),
          oldPassword: form.get("oldPassword"),
          newPassword: form.get("newPassword"),
        }

        console.log(credenptials)
        // const response = await axios.patch("/edit", credenptials);
        this.toast.fire({
          icon: "success",
          title: "Senha atualizada com sucesso",
          timer: 2000
        });
        return response.data;
      } catch (error) {
        await this.toast.fire({
          icon: "error",
          title: "Falha ao atualizar",
          text: error.message
        });
      }
    }
  }
}
</script>

<template>
  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="exampleModalLabel">Editando senha do usuário - '{{user.username}}'</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <form id="form-edit" class="w-100 d-flex flex-column gap-4">
            <div class="mb-4">
              <label for="email" class="form-label">Email:</label>
              <input type="email" class="form-control" id="email" name="email" required :value="user.email">
            </div>
            <div class="mb-4">
              <label for="oldPassword" class="form-label">Senha Atual:</label>
              <input type="password" class="form-control" id="oldPassword" name="password" required>
            </div>
            <div class="mb-4">
              <label for="newPassword" class="form-label">Senha Atual:</label>
              <input type="password" class="form-control" id="newPassword" name="password" required>
            </div>
          </form>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Fechar</button>
          <button type="button" class="btn btn-success" @click="edit">Salvar nova senha</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>