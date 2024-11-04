<template>
    <div class="multi-step-form">
      <!-- Progress Bar -->
      <div class="progress-bar">
        <div class="progress" :style="{ width: `${(currentStep / totalSteps) * 100}%` }"></div>
      </div>
  
      <!-- Form Card Container -->
      <div class="form-card" :key="currentStep">
        <transition name="fade">
          <component :is="getCurrentComponent" :formData="formData" @prev="prevStep" @next="nextStep" @submit="notsubmitForm" />
        </transition>
      </div>
    </div>
  </template>
  
  <script>
  import Step1 from '../components/MultistepForm/Step1.vue';
  import Step2 from '../components/MultistepForm/Step2.vue';
  import Step3 from '../components/MultistepForm/Step3.vue';
  import StepConfirmation from '@/components/MultistepForm/StepConfirmation.vue';
  
  export default {
    components: { Step1, Step2, Step3 },
    data() {
    return {
      currentStep: 1,
      totalSteps: 4,
      formData: {
        name: '',
        surname: '',
        age: '',
        address: '',
        selectedOption: '',
        additionalInfo: '',
      },
    };
    },
    computed: {
    getCurrentComponent() {
      switch (this.currentStep) {
        case 1:
          return Step1;
        case 2:
          return Step2;
        case 3:
          return Step3;
        case 4:
          return StepConfirmation; // Confirmation step
        default:
          return Step1;
      }
    },
  },
  methods: {
    nextStep(updatedData) {
      this.formData = { ...this.formData, ...updatedData };
      if (this.currentStep < this.totalSteps) this.currentStep++;
    },
    prevStep() {
      if (this.currentStep > 1) this.currentStep--;
    },
  }
};  
  </script>
  
  <style scoped>
  .multi-step-form {
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: var(--background);
    padding: 20px;
    min-height: 100vh;
  }
  
  .progress-bar {
    width: 100%;
    height: 8px;
    background: var(--lightgristrigo);
    border-radius: 4px;
    margin-bottom: 20px;
    overflow: hidden;
  }
  
  .progress {
    height: 100%;
    background-color: var(--verttrigo);
    transition: width 0.3s ease;
  }
  
  .form-card {
    width: 100%;
    max-width: 400px;
    background: white;
    padding: 20px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    transition: transform 0.3s ease;
    border: 1px solid var(--lightgristrigo);
  }
  </style>
  