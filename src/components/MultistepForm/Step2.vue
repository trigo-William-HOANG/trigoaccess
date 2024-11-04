<template>
    <div class="step-button-choice">
      <h2>Step 2: Choose an Option</h2>
      <div class="button-options">
        <button
          v-for="option in options"
          :key="option"
          :class="{ selected: selectedOption === option }"
          @click="selectOption(option)"
        >
          {{ option }}
        </button>
      </div>
      <div class="button-group">
        <button @click="$emit('prev')" class="prev-button">Previous</button>
        <button @click="nextStep" class="next-button" :disabled="!selectedOption">Next</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['formData'],
    data() {
      return {
        options: ['Option 1', 'Option 2', 'Option 3', 'Option 4'],
        selectedOption: this.formData.selectedOption || null,
      };
    },
    methods: {
      selectOption(option) {
        this.selectedOption = option;
      },
      nextStep() {
        this.$emit('next', { selectedOption: this.selectedOption });
      },
    },
    watch: {
      selectedOption(newVal) {
        this.$emit('update:formData', { ...this.formData, selectedOption: newVal });
      },
    },
  };
  </script>
  
  <style scoped>
  .step-button-choice {
    display: flex;
    flex-direction: column;
  }
  
  h2 {
    color: var(--bleutrigo);
  }
  
  .button-options {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-bottom: 20px;
  }
  
  button {
    padding: 10px 20px;
    border: 1px solid var(--lightgristrigo);
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    background-color: var(--background);
    color: var(--darkgristrigo);
  }
  
  button.selected {
    background-color: var(--verttrigo);
    color: white;
  }
  
  button:hover {
    background-color: var(--lightgristrigo);
  }
  
  .button-group {
    display: flex;
    justify-content: space-between;
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
  