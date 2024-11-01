<template>
  <Navbar/>
  <div v-if="loading" class="d-flex justify-content-center align-items-center vh-100">
    <div class="spinner-border" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
  </div>
  <div v-if="!loading" class="container mt-5">
    <ModalEditUser :user="user"/>
    <table class="table table-hover">
      <thead>
      <tr>
        <th scope="col">ID</th>
        <th scope="col">Nome</th>
        <th scope="col">Email</th>
        <th scope="col">CEP</th>
        <th scope="col">Endereço</th>
        <th scope="col">Cidade</th>
        <th scope="col">Ação</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="user in users" :key="user.id">
        <td> {{ user.id }}</td>
        <td> {{ user.username }}</td>
        <td> {{ user.email }}</td>
        <td> {{ user.address?.zipCode }}</td>
        <td> {{ formatAddressStreet(user.address) }}</td>
        <td> {{ formatAddressCity(user.address) }}</td>
        <td>
          <div class="btn-group">
            <button type="button" class="btn btn-light dropdown-toggle" data-bs-toggle="dropdown"
                    aria-expanded="false">
              Ação
            </button>
            <ul class="dropdown-menu">
              <li><a class="dropdown-item " data-bs-toggle="modal" data-bs-target="#exampleModal"
                     @click="handleEditUser(user)">Editar Senha</a></li>
              <li v-if="false"><a class="dropdown-item bg-danger text-bg-danger"
                                  @click="handleDeleteUser(user)">Excluir</a>
              </li>
            </ul>
          </div>
        </td>
      </tr>
      </tbody>
    </table>
  </div>

</template>

<script>
import Swal from "sweetalert2";
import {axios} from "../main.js";
import ModalEditUser from "../components/ModalEditUser.vue";
import Navbar from "../components/Navbar.vue";
import dados from "/src/Pages/user-data.json";


export default {
  name: "Users",
  components: {Navbar, ModalEditUser},
  data() {
    return {
      users: dados,
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
      user: {},
      modal: '',
      loading: false
    };
  },
  async mounted() {
    if (!localStorage.getItem("token")) {
      this.$router.push('/');
    }
    this.loading = true;
    this.toast.fire({
      icon: "info",
      title: "Aviso",
      text: "Você só poderá atualizar as senhas do usuário que acabou de logar.",
      timer: 3000
    });
    this.users = await this.getUsers();
  },
  methods: {
    async getUsers() {
      try {
        this.loading = true;
        //const response = await axios.get('/src/Pages/user-data.json');
        //return response.data;
        return dados;
      } catch (error) {
        await this.toast.fire({
          icon: "error",
          title: "Houve um erro com a requisição",
          text: error.message
        });
      } finally {
        this.loading = false;
      }
    },
    formatAddressStreet(address) {
      if (!address) {
        return "-";
      }
      return address.street + ' , ' + address.neighborhood;
    },
    formatAddressCity(address) {
      if (!address) {
        return "-";
      }
      return address.city + ' - ' + address.state;
    },
    handleEditUser(user) {
      this.user = user;
    },
    handleDeleteUser(user) {
      Swal.fire({
        title: "Você tem certeza?",
        html: "Excluindo usuário '" + user.username + "' <br>Esta ação não poderá ser desfeita!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Sim"
      }).then((result) => {
        if (result.isConfirmed) {
          axios.post('urlapi', {});
        }
      });
    },
  }
};
</script>

<style scoped>
</style>
