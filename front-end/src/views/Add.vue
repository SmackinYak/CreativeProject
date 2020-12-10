<template>
  <div class="add">
    <h1>
      Add Expense
    </h1>
    <form class="pure-form" @submit.prevent="addTicket">
      <fieldset class="form">
        <div class="inputContainer">
          <input class="input" type="text" v-model="description" />
          <input class="input" type="number" v-model.number="amount" />
        </div>
        <br />
        <button class="pure-button pure-button-primary" type="submit"  @click.prevent="addExpense">Add Expense</button>
      </fieldset>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Dashboard',
    data() {
    return {
      description: '',
      amount: 0,
      expenses: [],
    }
  },
  methods: {
    async addExpense() {
      try {
        if (this.description !== '' && this.amount !== 0) {
          await axios.post("/api/tickets", {
            description: this.description,
            amount: this.amount,
          });
          this.description = "";
          this.amount = "";
          return true;
        }
      } catch (error) {
        console.log(error);
      }
    }
  }
}
</script>

<style scoped>
.add {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.input {
  height: 30px;
}

.pure-form {
  
}

.inputContainer {
  display: flex;
  justify-content: space-around;
}

.form {
  width: 500px;
  display: flex;
  flex-direction: column;
}

</style>