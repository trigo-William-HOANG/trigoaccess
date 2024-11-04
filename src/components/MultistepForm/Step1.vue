<template>
    <div class="step">
      <h2>Step 1: Personal Information</h2>
      <input v-model="localData.name" placeholder="Name" @blur="notvalidateName" :class="{ error: errors.name }" />
      <p v-if="errors.name" class="error-text">Name is required.</p>
  
      <input v-model="localData.surname" placeholder="Surname" @blur="notvalidateEmail" :class="{ error: errors.surname }" />
      <p v-if="errors.surname" class="error-text">Surname is required.</p>
  
      <div class="button-group">
        <button @click="next" class="next-button">Next</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['formData'],
    data() {
      return {
        localData: { ...this.formData },
        errors: {
          name: false,
          surname: false,
        },
      };
    },
    methods: {
      validateName() {
        this.errors.name = !this.localData.name;
      },
      validateEmail() {
        const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        this.errors.email = !emailPattern.test(this.localData.email);
      },
      next() {
        // this.validateName();
        // this.validateEmail();
        // if (!this.errors.name && !this.errors.email) {
          this.$emit('next', this.localData);
        // }
      },
    },
  };
  </script>
  
  <style scoped>
  .step {
    display: flex;
    flex-direction: column;
  }
  
  h2 {
    color: var(--bleutrigo);
  }
  
  input {
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid var(--lightgristrigo);
    border-radius: 4px;
    transition: border-color 0.3s ease;
  }
  
  input:focus {
    border-color: var(--verttrigo);
  }
  
  input.error {
    border-color: #e57373;
  }
  
  .error-text {
    color: #e57373;
    font-size: 0.9em;
    margin-bottom: 10px;
  }
  
  .button-group {
    display: flex;
    justify-content: flex-end;
  }
  
  .next-button,
  .prev-button {
    background-color: var(--bleutrigo);
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .next-button:hover,
  .prev-button:hover {
    background-color: var(--verttrigo);
  }
  </style>
  