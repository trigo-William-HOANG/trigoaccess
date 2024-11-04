<template>
    <div class="step-confirmation">
      <h2>Review Your Information</h2>
      <div class="review-section">
        <p><strong>Name:</strong> {{ formData.name }}</p>
        <p><strong>Surname:</strong> {{ formData.Surname }}</p>
        <p><strong>Selected Option:</strong> {{ formData.selectedOption }}</p>
        <p v-if="formData.additionalInfo"><strong>Additional Info:</strong> {{ formData.additionalInfo }}</p>
      </div>
      <div class="button-group">
        <button @click="$emit('prev')" class="prev-button">Previous</button>
        <button @click="confirmSubmission" class="confirm-button">Confirm</button>
      </div>
  
      <div v-if="submitted" class="success-message">
        <p>Your form has been successfully submitted! Thank you.</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['formData'],
    data() {
      return {
        submitted: false,
      };
    },
    methods: {
      confirmSubmission() {
        // Imitation d'envoie de formulaire 
        console.log('Form data:', this.formData);
        this.submitted = true;
        setTimeout(() => {
        window.location.href = '/test';  
        }, 2000);
      },
    },
  };
  </script>
  
  <style scoped>
  .step-confirmation {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  h2 {
    color: var(--bleutrigo);
    margin-bottom: 20px;
  }
  
  .review-section {
    width: 100%;
    max-width: 400px;
    background-color: var(--lightgristrigo);
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
  }
  
  p {
    color: var(--darkgristrigo);
    margin: 8px 0;
  }
  
  .button-group {
    display: flex;
    gap: 10px;
  }
  
  .prev-button,
  .confirm-button {
    padding: 10px 20px;
    border-radius: 4px;
    cursor: pointer;
    border: none;
    transition: background-color 0.3s ease;
  }
  
  .prev-button {
    background-color: var(--bleutrigo);
    color: white;
  }
  
  .confirm-button {
    background-color: var(--verttrigo);
    color: white;
  }
  
  .prev-button:hover {
    background-color: #00345a;
  }
  
  .confirm-button:hover {
    background-color: #8cb644;
  }
  
  .success-message {
    margin-top: 20px;
    padding: 10px 20px;
    color: white;
    background-color: var(--verttrigo);
    border-radius: 4px;
    text-align: center;
  }
  </style>
  