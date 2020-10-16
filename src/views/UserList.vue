<template>
  <div>
    <table v-if="users && users.length" class="uk-table uk-table-striped">
      <thead>
      <tr>
        <th>ФИО</th>
        <th>Телефон</th>
        <th>eMail</th>
        <th>Страна</th>
        <th>Город</th>
        <th>Улица</th>
        <th>Квартира</th>
        <th></th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="user in users"
          :key="user.id"
      >
        <td v-text="user.name"></td>
        <td v-text="user.phone"></td>
        <td v-text="user.email"></td>
        <td v-text="user.country"></td>
        <td v-text="user.city"></td>
        <td v-text="user.street"></td>
        <td v-text="user.apartment"></td>
        <td><span @click="Edit(user.id)" uk-toggle="target: #edit-form" class="icon" uk-icon="icon: pencil"></span></td>
      </tr>
      </tbody>
    </table>
    <template v-else>
      <h4>Ни одного пользователя не зарегистрированно...</h4>
      <router-link :to="{name: 'Register'}">Пройти регистрацию</router-link>
    </template>

    <div id="edit-form" uk-modal>
      <div class="uk-modal-dialog uk-modal-body">
        <div v-if="editUser" class="edit-wrapper">
          <div class="edit-panel">
            <div class="uk-margin">
              <label class="uk-form-label"
                     for="form-stacked-name">ФИО <span class="required"><sup>*</sup></span></label>
              <div class="uk-form-controls">
                <input v-model="editUser.name"
                       class="uk-input"
                       id="form-stacked-name"
                       type="text"
                       @blur="$v.editUser.name.$touch()"
                       :class="{'uk-form-danger': $v.editUser.name.$error}">
              </div>
            </div>

            <div class="uk-margin">
              <label class="uk-form-label"
                     for="form-stacked-phone">Номер телефона <span class="required"><sup>*</sup></span></label>
              <div class="uk-form-controls">
                <input v-model="editUser.phone"
                       class="uk-input"
                       id="form-stacked-phone"
                       type="tel"
                       @blur="$v.editUser.phone.$touch()"
                       :class="{'uk-form-danger': $v.editUser.phone.$error}">
              </div>
            </div>

            <div class="uk-margin">
              <label class="uk-form-label"
                     for="form-stacked-email">Email <span class="required"><sup>*</sup></span></label>
              <div class="uk-form-controls">
                <input v-model="editUser.email"
                       class="uk-input"
                       id="form-stacked-email"
                       type="email"
                       @blur="$v.editUser.email.$touch()"
                       :class="{'uk-form-danger': $v.editUser.email.$error}">
              </div>
            </div>

            <div uk-grid>
              <div class="uk-width-1-2">
                <label class="uk-form-label"
                       for="form-stacked-country">Страна</label>
                <div class="uk-form-controls">
                  <input v-model="editUser.country"
                         class="uk-input"
                         id="form-stacked-country"
                         type="text">
                </div>
              </div>

              <div class="uk-width-1-2">
                <label class="uk-form-label"
                       for="form-stacked-city">Город</label>
                <div class="uk-form-controls">
                  <input v-model="editUser.city"
                         class="uk-input"
                         id="form-stacked-city"
                         type="text">
                </div>
              </div>
            </div>

            <div uk-grid>
              <div class="uk-width-1-2">
                <label class="uk-form-label"
                       for="form-stacked-street">Улица</label>
                <div class="uk-form-controls">
                  <input v-model="editUser.street"
                         class="uk-input"
                         id="form-stacked-street"
                         type="text">
                </div>
              </div>

              <div class="uk-width-1-2">
                <label class="uk-form-label"
                       for="form-stacked-apartment">Квартира</label>
                <div class="uk-form-controls">
                  <input v-model="editUser.apartment"
                         class="uk-input"
                         id="form-stacked-apartment"
                         type="text">
                </div>
              </div>
            </div>
            <div class="uk-margin">
              <button class="uk-button uk-button-danger uk-modal-close uk-margin-right"
                      @click="closeUserEdit">
                Отменить
              </button>
              <button type="submit"
                      class="uk-button uk-modal-close uk-button-primary"
                      @click="saveUserEdit"
                      :disabled="disabledBtn"
              >
                Сохранить
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
    import {required, minLength, helpers} from 'vuelidate/lib/validators'

    const alphaEmail = helpers.regex('alpha', /^[^@\s]+@[^@\s]+\.[^@\s]+$/)
    const alphaPhone = helpers.regex('alpha', /^([+]?[\s0-9]+)?(\d{3}|[(]?[0-9]+[)])?([-]?[\s]?[0-9])+$/)

    export default {
        name: "UserList",
        data: () => ({
            users: {},
            UserIndex: null,
            editUser: []
        }),
        mounted() {
            this.getData();
        },
        computed: {
            disabledBtn() {
                return this.$v.editUser.name.$invalid ||
                    this.$v.editUser.phone.$invalid ||
                    this.$v.editUser.email.$invalid
            }
        },
        methods: {
            getData() {
                if (localStorage.getItem('users')) {
                    this.users = JSON.parse(localStorage.getItem('users'));
                }
            },
            Edit(id) {
                this.editUser = null;
                this.UserIndex = null;
                this.UserIndex = this.users.findIndex(el => el.id == id);
                if (this.UserIndex == -1) {
                    return false;
                } else {
                    this.editUser = Object.assign({}, this.users[this.UserIndex])
                }
            },
            closeUserEdit() {
                this.editUser = null
            },
            saveUserEdit() {
                this.users.forEach((el, index) => {
                    if (el.id == this.editUser.id) {
                        this.users[index] = this.editUser
                    }
                })
                // очистить
                localStorage.clear();
                localStorage.setItem('users', JSON.stringify(this.users));
                this.editUser = null;
                this.getData();
            }
        },
        validations: {
            editUser: {
                name: {
                    required
                },
                phone: {
                    required,
                    minLength: minLength(4),
                    alpha: alphaPhone
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
  .icon {
    width: 32px;
    cursor: pointer;
    transition: all 0.1s ease;
  }

  .icon:hover {
    color: #2c3e50;
    transform: scale(1.1);
  }

  .required {
    color: red;
  }
</style>