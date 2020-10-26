<template>
    <div class="w-100 h-100">
      <div class="w-100 h-100 d-flex flex-column justify-content-center align-items-center">
        <div class="w-25">
            <h1 class="text-white display-4">R$ {{balance}}</h1>
            <form @submit.prevent="saveData">
                <div class="input-group mb-3 w-100">
                    <select v-model="form.type" class="form-control form-control-lg">
                        <option value="deposit">Depósito</option>
                        <option value="withdraw">Saque</option>
                    </select>
                    <input v-model="form.amount" :class="{'is-invalid':form.errors.has('amount')}" type="number" step="0.01" class="form-control form-control-lg" aria-label="Recipient's username" aria-describedby="button-addon2"
                    @keydown="form.errors.clear('amount')"
                    >
                    <div class="input-group-append">
                        <button class="btn btn-success" type="submit" id="button-addon2">Cadastrar</button>
                    </div>
                </div>
                <span class="text-danger pt-3 pb-3" style="font-size: 20px;" v-if="form.errors.has('amount')" v-text="form.errors.get('amount')"></span>
            </form>
            <div class="w-25">
                <div v-for="transaction in transactions" :key="transaction.id" class="w-100">
                    <!-- {{transaction.amount}} -->
                </div>
            </div>
        </div>
      </div>
    </div>
</template>

<script scoped>
    import Form from "../assets/form";
    import axios from "axios";

    export default {
        data() {
            return {
                transactions:'',
                balance:'',
                form: new Form({
                    amount: '',
                    type: 'deposit',
                })
            }
        },
        methods: {
            checkLogin() {
                let data = new FormData();
                data.append('hash', localStorage.getItem('client_hash'));
                axios.post(process.env.VUE_APP_API_URL + '/api/check-login', data).then((res) => {
                    console.log(res);
                    this.getTransactions();
                }).catch((error) => {
                    console.log(error);
                });
            },
            getTransactions() {
                axios.get(process.env.VUE_APP_API_URL + '/api/transaction/' + localStorage.getItem('client_hash')).then((res) => {
                    this.transactions = res.data.transactions;
                    this.balance = res.data.balance;
                }).catch((error) => {
                    console.log(error);
                })
            },
            saveData() {
                let data = new FormData();
                data.append('amount', this.form.amount);
                data.append('type', this.form.type);
                data.append('hash', localStorage.getItem('client_hash'));
                axios.post(process.env.VUE_APP_API_URL + '/api/transaction', data).then(() => {
                    this.form.amount = '';
                    this.form.type = 'deposit';
                    this.getTransactions();
                }).catch((error) => {
                    console.log(error);
                });
            }
        },
        mounted() {
            this.checkLogin();
        }
    }
</script>
