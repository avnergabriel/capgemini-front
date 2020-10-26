<template>
    <div class="dashboard">
        <TransactionForm />
    </div>
</template>

<script>
// @ is an alias to /src
import TransactionForm from "@/components/TransactionForm.vue";

export default {
  name: "dashboard",
  components: {
    TransactionForm
  }
};
</script>
