<template>
  <form class="card auth-card" 
  style="
  border-radius: 10px;
  padding: 10px 10px 20px;"
  @submit.prevent="submitHandler">
    <div class="card-content">
      <span class="card-title" style="text-align: center">Домашняя бухгалтерия</span>
      <div class="input-field">
        <input
            id="email"
            type="text"
            v-model.trim="email"
            :class="{invalid: ($v.email.$dirty && !$v.email.required) || ($v.email.$dirty && !$v.email.email)}"
        >
        <label for="email">Email</label>
        <small 
          class="helper-text invalid"
          v-if="$v.email.$dirty && !$v.email.required"
        >Поле email не должно быть пустым</small>
        <small 
          class="helper-text invalid"
          v-else-if="$v.email.$dirty && !$v.email.email"
        >Введите корректный email</small>
      </div>
      <div class="input-field">
        <input
            id="password"
            type="password"
            v-model.trim="password"
            :class="{invalid: ($v.password.$dirty && !$v.password.required) || ($v.password.$dirty && !$v.password.minLength)}"
        >
        <label for="password">Пароль</label>
        <small 
          class="helper-text invalid"
          v-if="$v.password.$dirty && !$v.password.required"
        >Введите пароль</small>
        <small 
          class="helper-text invalid"
          v-else-if="$v.password.$dirty && !$v.password.minLength"
        >Пароль должен быть больше {{$v.password.$params.minLength.min}} символов. Сейчас он {{password.length}} </small>
      </div>
      <div class="input-field">
        <input
            id="name"
            type="text"
            v-model.trim="name"
            :class="{invalid: $v.name.$dirty && !$v.name.required}"
        >
        <label for="name">Имя</label>
        <small 
        class="helper-text invalid"
        v-if="$v.name.$dirty && !$v.name.required"
        >Введите ваше имя</small>
      </div>
      <p>
        <label>
          <input type="checkbox" v-model="agree" />
          <span>С правилами согласен</span>
        </label>
      </p>
    </div>
    <div class="card-action">
          <button
              class="btn waves-effect light-blue darken-2 right" style="text-transform: none;"
              type="submit"
          > Регистрация
          </button>

      <p class="left light-blue-text text-darken-2">
        <router-link class="light-blue-text text-darken-2 left" style="text-transform: none;" to="/login">Войти</router-link>
      </p>
    </div>
  </form>
</template>

<script>
import {email, required, minLength} from 'vuelidate/lib/validators'

export default {
  name: 'register',
  data: () => ({
    email: '',
    password: '',
    name: '',
    agree: false
  }),
  validations: {
    email: {email, required},
    password: {required, minLength: minLength(6)},
    name: {required},
    agree: {checked: v => v}
  },
  methods: {
    submitHandler() {
      if (this.$v.$invalid) {
        this.$v.$touch()
        return
      }

      const formData = {
        email: this.email,
        password: this.password,
        name: this.name
      }

      console.log(formData)

      this.$router.push('/')
    }
  }
}
</script>
