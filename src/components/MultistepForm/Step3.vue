<template>
    <div class="step-button-choice">
      <h2>Step 3: Choose an Option Dynamic (First Button)</h2>
  
      <!-- Option primaire  -->
      <div class="button-options">
        <button
          v-for="option in options"
          :key="option.label"
          :class="{ selected: selectedOption === option.label }"
          @click="selectOption(option.label)"
        >
          {{ option.label }}
        </button>
      </div>
  
      <!-- Question avec un rendu conditionnel en fonction de l'option primaire choisi -->
      <div v-if="selectedOption === 'Option 1'" class="conditional-question">
        <label for="additionalInfo">Please provide additional information:</label>
        <input
          v-model="additionalInfo"
          id="additionalInfo"
          placeholder="Additional info for Option 1"
        />
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
        options: [
          { label: 'Option 1' },
          { label: 'Option 2' },
          { label: 'Option 3' },
          { label: 'Option 4' },
        ],
        selectedOption: this.formData.selectedOption || null,
        additionalInfo: this.formData.additionalInfo || '',
      };
    },
    methods: {
      selectOption(option) {
        this.selectedOption = option;
        // Contenu Additionnel
        if (option !== 'Option 1') {
          this.additionalInfo = '';
        }
      },
      nextStep() {
        this.$emit('next', {
          selectedOption: this.selectedOption,
          additionalInfo: this.additionalInfo,
        });
      },
    },
    watch: {
      selectedOption(newVal) {
        this.$emit('update:formData', { ...this.formData, selectedOption: newVal });
      },
      additionalInfo(newVal) {
        this.$emit('update:formData', { ...this.formData, additionalInfo: newVal });
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
  
  .conditional-question {
    margin-top: 20px;
  }
  
  input {
    padding: 10px;
    margin-top: 5px;
    border: 1px solid var(--lightgristrigo);
    border-radius: 4px;
    width: 100%;
  }
  
  .button-group {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
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
  