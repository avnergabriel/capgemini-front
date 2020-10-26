<template>
    <div class="container">
        <h1 class="text-white display-4">Register</h1>
        <br><br>
        <div class="row">
            <div class="col-md-3">
            </div>
            <div class="col-md-6">
                <form @submit.prevent="register">
                    <div class="form-group">
                        <input type="text" class="form-control" required v-model="name" placeholder="Nome completo">
                    </div>
                    <div class="form-group">
                        <input type="text" class="form-control" required v-model="bank_account" placeholder="Conta bancária">
                    </div>
                    <div class="form-group">
                        <input type="password" class="form-control" required v-model="password" placeholder="Senha">
                        <div v-if="password.lenght > 1 && password.lenght < 6" class="text-danger">
                            Senha curta
                        </div>
                    </div>
                    <div class="form-group">
                        <input type="password" class="form-control" id="reenterPassword" required v-model="reenterPassword" placeholder="Confirmar senha">
                        <div v-if="password.lenght > 1 && password != reenterPassword" class="text-danger">
                            Senha não confere
                        </div>
                    </div>
                    <br>
                    <button type="submit" class="btn-lg btn-primary">Login</button>
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
            name: "",
            bank_account: "",
            password: "",
            reenterPassword: "",
        }
    },
    methods : {
        register : function(){
            let data = new FormData();
            data.append('name', this.name);
            data.append('bank_account', this.bank_account);
            data.append('password', this.password);
            axios.post(process.env.VUE_APP_API_URL + '/api/register', data).then((res) => {
                console.log(res.data.data);
                this.$router.push('login');
            }).catch((error) => {
                console.log(error);
            });
        }
    }
}
</script>