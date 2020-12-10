<template>
<div class="main">
<div class="header">
  <h1>My Expenses</h1>
  <h1 class="total">Total: ${{total.toFixed(2)}}</h1>
</div>
  <div>
</div>
<div class="expenseContainer">
<div v-for="expense in expenses" v-bind:key="expense.id">

    <div class="expense">
      <div>{{expense.description}}</div>
      <div class="rightText">
        <div>${{expense.amount.toFixed(2)}}</div>
        <div class="delete" @click="deleteExpense(expense._id)">X</div>
      </div>
    </div>
  </div>
</div>
</div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
export default {
  name: 'Dashboard',
    data() {
    return {
      expenses: [],
      total: 0,
    }
  },
  computed: {
    user() {
      return this.$root.$data.user;
    }
  },
  created() {
    this.getTickets();
  },
  methods: {
    async getTickets() {
      try {
        console.log('Getting tickets...')
        let response = await axios.get("/api/tickets");
        this.expenses = response.data.tickets;
        this.total = 0;
        for (let i = 0; i < this.expenses.length; i++) {
          this.total += this.expenses[i].amount;
        }
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    async deleteExpense(id) {
      try {
        await axios.delete("/api/tickets/" + id);
        this.getTickets();
      } catch (error) {
        console.log(error);
      }
    },
    time(d) {
      return moment(d).format('D MMMM YYYY, h:mm:ss a');
    },
  }
}
</script>

<style scoped>
.header {
  display: flex;
}

.total {
  margin-left: 18px;
}

textarea {
  width: 100%;
  max-width: 500px;
}

.expenseContainer {
  display: grid;
  grid-template-columns: repeat(auto-fill,minmax(400px, 1fr));
  width: 100%;
}

.rightText {
  display: flex;
  align-items: center;
}

.delete {
  margin-left: 16px;
  background-color: red;
  width: 50px;
  height: 50px;
  border-radius: 0 12px 12px 0;
  justify-content: center;
  display: flex;
  align-items: center;
  font-weight: 800;
  color: white;
}

.delete:hover {
  color: gray;
  cursor: pointer;
}

.expense {
  justify-content: space-between;
  font-size: 18px;
  height: 50px;
  background-color: white;
  flex: 1 1 400px;
  padding: 0 0 0 16px;
  display: flex;
  align-items: center;
  margin: 12px;
  border-radius: 12px;
}

h3 {
  font-size: 12px;
  font-weight: normal;
  background-color: #ccc;
  padding: 10px 20px;
}


label {
  background-color: #000;
  color: white;
  padding: 5px;
  border-radius: 30px;
  font-size: 12px;
  margin-right: 10px;
}
</style>