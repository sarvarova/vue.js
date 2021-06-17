<template>
  <div class="login-page-component">
    <div class="content">
      <form class="form" @submit.prevent="handleSubmit">
        <div class="form-title">Авторизация</div>
        <div class="row">
          <app-input 
            title="Логин" 
            v-model="user.name" 
            icon="user"
            :errorMessage="validation.firstError('user.name')" />
        </div>
        <div class="row">
          <app-input
           title="Пароль"
           v-model="user.password" 
           icon="key" type="password"
           :errorMessage="validation.firstError('user.password')" />
        </div>
        <div class="btn">
          <app-button :disabled="isSubminDisabled" title="Отправить" />
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import appInput from "../../components/input";
import appButton from "../../components/button";
import { Validator, mixin as ValidatorMixin } from "simple-vue-validator";
import $axios from "../../requests";
import { mapActions } from "vuex";

//const baseUrl = "https://webdev-api.loftschool.com/"

export default {
  mixins: [ValidatorMixin],
  validators: {
    "user.name": (value) => {
      return Validator.value(value).required("Введите имя пользователя");
    },
    "user.password": (value) => {
      return Validator.value(value).required("Введите пароль");
    },
  },
  data: () => ({
    user: {
      name: "",
      password: "",
    },
    isSubminDisabled: false,
  }),
  components: { appInput, appButton },
  methods: {
    ...mapActions({
      showTooltip: "tooltips/show",
      login: "user/login"
    }),

    async handleSubmit() {
      if (await this.$validate() === false) return;
      this.isSubminDisabled = true;
      try {
        const response = await $axios.post('/login', this.user);

        const token = response.data.token;
        localStorage.setItem("token", token);
        $axios.defaults.headers["authorization"] = `Bearer ${token}`;
        
        const userResponse = await $axios.get("/user");
        this.login(userResponse.data.user);
        this.$router.replace('/');
        } catch (error) {
          this.showTooltip({
          text: error.response.data.error,
          type: "error"
        })   
        } finally {
          this.isSubminDisabled = false;
        }        
      }
    }
  };
</script>

<style lang="postcss" scoped src="./login.pcss"></style>