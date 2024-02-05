<script setup>
    import Header from './components/Header.vue';
    import Balance from './components/Balance.vue';
    import IncomeExpenses from './components/IncomeExpenses.vue';
    import AddTransaction from './components/AddTransaction.vue';
    import {ref, computed} from 'vue'

    const transactions = ref([])
    
    //get the total
    const total = computed(() => {
        return transactions.value.reduce( (acc, transaction) => {
            return acc + transaction.amount
        }, 0)
    })

    //get the income by adding all positive values
    const income = computed( () => {
        return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce( (acc, transaction) => {
            return acc + transaction.amount
        }, 0)
    })

    //handle transaction submitted
    const handleTransactionSubmitted = (transactionData) => {
        transactions.value.push({
            text: transactionData.text,
            amount: transactionData.amount,
        })
    }

    //get the expenses by adding all negative values
    const expense = computed( () => {
        return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce( (acc, transaction) => {
            return acc + transaction.amount
        }, 0)
    })

</script>

<template>
    <Header></Header>
    <div class="container">
        <Balance :total="total"></Balance>
        <IncomeExpenses :income="income" :expense="expense"></IncomeExpenses>
        <AddTransaction @transactionSubmitted="handleTransactionSubmitted"></AddTransaction>
        <!-- {{  transactions }} -->
    </div>
</template>