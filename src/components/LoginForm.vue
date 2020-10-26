<template>
    <div class="container">
        <h1 class="text-white display-4">Login</h1>
        <br><br>
        <div class="row">
            <div class="col-md-3">
            </div>
            <div class="col-md-6">
                <form @submit.prevent="login">
                    <div class="form-group">
                        <input type="text" class="form-control" required v-model="bank_account" placeholder="Conta corrente">
                    </div>
                    <div class="form-group">
                        <input type="password" class="form-control" required v-model="password" placeholder="Senha">
                    </div>
                    <button type="submit" class="btn btn-primary">Login</button>
                </form>
            </div>
            <div class="col-md-3">
            </div>
        </div>
    </div>
</template>

<script>
    import axios from "axios";

    export default {
        data() {
            return {
                bank_account: "",
                password: ""
            }
        },
        methods : {
            login : function(){
                let data = new FormData();
                data.append('bank_account', this.bank_account);
                data.append('password', this.password);
                axios.post(process.env.VUE_APP_API_URL + '/api/login', data).then((res) => {
                    localStorage.setItem('client_hash', res.data.data);
                    this.bank_account = '';
                    this.password = '';
                    window.location.href = 'dashboard';
                    // this.$router.push('dashboard');
                }).catch((error) => {
                    console.log(error);
                });
            }
        }
    }
</script>

<style scoped>

</style>