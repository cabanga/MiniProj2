<template>
  <section class="page-section">
    <b-container>
      <HeaderPage title="Registo" />

      <b-row>
        <b-col cols="4"></b-col>
        <b-col cols="4">
          <form @submit.prevent="register">
            <div class="form-group">
              <input
                v-model="name"
                type="text"
                class="form-control form-control-lg"
                id="txtName"
                placeholder="Seu nome"
                required
              />
            </div>
            <div class="form-group">
              <input
                v-model="birth_date"
                type="text"
                onmouseenter="(this.type='date')"
                onmouseleave="(this.type='text')"
                class="form-control form-control-lg"
                id="txtBirthDate"
                placeholder="Sua data de nascimento"
                required
              />
            </div>
            <div class="form-group">
              <input
                v-model="location.city"
                type="text"
                class="form-control form-control-lg"
                id="txtCity"
                placeholder="Cidade onde vives"
                required
              />
            </div>
            <div class="form-group">
              <input
                v-model="location.country"
                type="text"
                class="form-control form-control-lg"
                id="txtCountry"
                placeholder="Seu país"
                required
              />
            </div>
            <div class="form-group">
              <input
                v-model="auth.username"
                type="text"
                class="form-control form-control-lg"
                id="txtEmail"
                placeholder="Escolha um username"
                required
              />
            </div>
            <div class="form-group">
              <input
                v-model="auth.password"
                type="password"
                class="form-control form-control-lg"
                id="txtPassword"
                placeholder="Sua senha"
                required
              />
            </div>
            <div class="form-group">
              <input
                type="password"
                class="form-control form-control-lg"
                id="txtConfirmPassword"
                placeholder="confirma senha"
              />
            </div>
            <button type="submit" class="btn btn-success mr-2">
              <i class="fas fa-file-signature"></i> Entrar
            </button>
            <router-link
              :to="{ name: 'login' }"
              tag="button"
              class="btn btn-danger"
            >
              <i class="fas fa-window-close"></i>CANCELAR</router-link
            >
          </form>
        </b-col>
        <b-col cols="4"></b-col>
      </b-row>
    </b-container>
  </section>
</template>

<script>
import { AUTH_REGISTER } from "@/store/auth/auth.constants";
import HeaderPage from "@/components/HeaderPage.vue";
import router from "../router";
import { mapGetters } from "vuex";

export default {
  name: "Register",
  components: {
    HeaderPage
  },
  data: function() {
    return {
      location: { city: "", district: "", country: "" },
      auth: { username: "", password: "" },
      gamification: { points: 0, quiz: 0 },
      active: true,
      name: "",
      type: "user",
      birth_date: "",
      description: ""
    };
  },
  computed: {
    ...mapGetters("auth", ["getMessage"])
  },
  methods: {
    register() {
      if (
        document.querySelector("#txtPassword").value !==
        document.querySelector("#txtConfirmPassword").value
      ) {
        this.$alert(
          "As senhas não concidem, por favor, digite novamente",
          "Erro de validação do formulário",
          "error"
        );
      } else {
        this.$store
          .dispatch(`auth/${AUTH_REGISTER}`, this.$data)
          .then(() => {
            this.$alert(this.getMessage, "Registo", "success");
            router.push({ name: "home" });
          })
          .catch(err => {
            this.$alert(err.message, "Algo deu errado", "error");
          });
      }
    }
  }
};
</script>
