<template>
  <form @submit.prevent="registerUser"
        class="uk-width-1-2"
        novalidate>
    <fieldset class="uk-fieldset">

      <legend class="uk-legend">Регистрация</legend>

      <div class="uk-margin">
        <label class="uk-form-label"
               for="form-stacked-name">ФИО</label>
        <div class="uk-form-controls">
          <input v-model="formReg.name"
                 class="uk-input"
                 id="form-stacked-name"
                 type="text"
                 @blur="$v.formReg.name.$touch()"
                 :class="{'uk-form-danger': $v.formReg.name.$error}"
                 placeholder="Иванов Иван Иванович">
        </div>
      </div>

      <div class="uk-margin">
        <label class="uk-form-label"
               for="form-stacked-phone">Номер телефона</label>
        <div class="uk-form-controls">
          <input v-model="formReg.phone"
                 class="uk-input"
                 id="form-stacked-phone"
                 type="tel"
                 @blur="$v.formReg.phone.$touch()"
                 :class="{'uk-form-danger': $v.formReg.phone.$error}"
                 placeholder="+375290000000">
        </div>
      </div>

      <div class="uk-margin">
        <label class="uk-form-label"
               for="form-stacked-email">Email</label>
        <div class="uk-form-controls">
          <input v-model="formReg.email"
                 class="uk-input"
                 id="form-stacked-email"
                 type="email"
                 @blur="$v.formReg.email.$touch()"
                 :class="{'uk-form-danger': $v.formReg.email.$error}"
                 placeholder="user@site.by">
        </div>
      </div>

      <div uk-grid>
        <div class="uk-width-1-2">
          <label class="uk-form-label"
                 for="form-stacked-country">Страна</label>
          <div class="uk-form-controls">
            <input v-model="formReg.country"
                   class="uk-input"
                   id="form-stacked-country"
                   type="text"
                   placeholder="Беларусь">
          </div>
        </div>

        <div class="uk-width-1-2">
          <label class="uk-form-label"
                 for="form-stacked-city">Город</label>
          <div class="uk-form-controls">
            <input v-model="formReg.city"
                   class="uk-input"
                   id="form-stacked-city"
                   type="text"
                   placeholder="Минск">
          </div>
        </div>
      </div>

      <div uk-grid>
        <div class="uk-width-1-2">
          <label class="uk-form-label"
                 for="form-stacked-street">Улица</label>
          <div class="uk-form-controls">
            <input v-model="formReg.street"
                   class="uk-input"
                   id="form-stacked-street"
                   type="text"
                   placeholder="Маяковского">
          </div>
        </div>

        <div class="uk-width-1-2">
          <label class="uk-form-label"
                 for="form-stacked-apartment">Квартира</label>
          <div class="uk-form-controls">
            <input v-model="formReg.apartment"
                   class="uk-input"
                   id="form-stacked-apartment"
                   type="text"
                   placeholder="1">
          </div>
        </div>
      </div>
      <div class="uk-margin">
        <button type="submit"
                class="uk-button uk-button-default"
                :disabled="disabledBtn"
        >
          Зарегистрироваться
        </button>
      </div>

    </fieldset>
  </form>
</template>

<script>
    import {required, minLength, helpers} from 'vuelidate/lib/validators'

    const alphaEmail = helpers.regex('alpha', /^[^@\s]+@[^@\s]+\.[^@\s]+$/)

    export default {
        name: "register",
        data: () => ({
            formReg: {
                name: '',
                phone: '',
                email: '',
                country: '',
                city: '',
                street: '',
                apartment: '',
            }
        }),
        computed: {
            disabledBtn() {
                return this.$v.formReg.name.$invalid ||
                    this.$v.formReg.phone.$invalid ||
                    this.$v.formReg.email.$invalid
            }
        },
        methods: {
            registerUser(e) {
                console.log(e)
            },
        },
        validations: {
            formReg: {
                name: {
                    required
                },
                phone: {
                    required,
                    minLength: minLength(4)
                },
                email: {
                    required,
                    minLength: minLength(5),
                    alpha: alphaEmail
                }
            }
        }
    }
</script>

<style scoped>

</style>