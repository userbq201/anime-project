<template lang="pug">
    div#mainAuth
        div#auth-modal.modal.modal-auth
            div.modal-content.modal-content_auth
                div.header
                    h5 Авторизация
                    span
                        i.fa.fa-times.modal-close
                div.inputs
                    div.row
                        div.input-field.col.s12
                            input(type='text', name='login', id='auth-login_modal', v-model='email')
                            label(for='auth-login_modal') Имеил
                    div.row
                        div.input-field.col.s12
                            input(type='password', name='password', id='auth-password_modal', v-model='password')
                            label(for='auth-password_modal') Пароль
                    div.row.button
                        div.input-field.col.s12
                            a(href='#', class='button-auth waves-effect waves-light btn col s12', @click='register()') Авторизоваться
</template>

<script>
    import axios from 'axios';
    import { getToken, saveToken } from '../modules/token';

    export default {
        data() {
            return {
                email: '',
                password: ''
            }
        },
        methods: {
            register() {
                let self = this;
                if (this.email.length > 1 && this.password.length > 1) {
                    axios.post('/api/auth', {
                        data: {
                            email: this.email,
                            password: this.password
                        },
                        token: getToken()
                    }).then(function (response) {
                        saveToken(response.data.success.remember_token);
                        location.reload();
                    }).catch(function (error) {
                        Materialize.toast(JSON.stringify(error.response.data.error), 3000);
                    });
                }
            }
        }
    }
</script>