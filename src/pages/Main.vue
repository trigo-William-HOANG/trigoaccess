<template>
  <div class="multi-step-form">
    <!-- Progress Bar -->
    <div class="progress-bar">
      <div class="progress" :style="{ width: `${(currentStep / totalSteps) * 100}%` }"></div>
    </div>

    <!-- Form Card Container -->
    <div class="form-card" :key="currentStep">
      <transition name="fade">
        <component
          :is="getCurrentComponent"
          :formData="formData"
          @prev="prevStep"
          @next="nextStep"
          @updateSteps="updateSteps"
        />
      </transition>
    </div>
  </div>
</template>

<script>
import Step1 from '../components/MultistepForm/Step1.vue';
import Step2 from '../components/MultistepForm/Step2.vue';
import Step3 from '../components/MultistepForm/Step3.vue';
import Step4 from '../components/MultistepForm/Step4.vue';
import Step5 from '../components/MultistepForm/Step5.vue'; 
import StepConfirmation from '@/components/MultistepForm/StepConfirmation.vue';

export default {
  components: { Step1, Step2, Step3, Step4, Step5, StepConfirmation },
  data() {
    return {
      currentStep: 1,
      steps: ['Step1', 'Step2', 'Step3', 'Step4', 'Step5', 'StepConfirmation'], // Default step order
      formData: {
        selectedOption: null, // Selected option from Step1
      },
    };
  },
  computed: {
    totalSteps() {
      return this.steps.length;
    },
    getCurrentComponent() {
      const step = this.steps[this.currentStep - 1]; // Get the current step name
      switch (step) {
        case 'Step1':
          return Step1;
        case 'Step2':
          return Step2;
        case 'Step3':
          return Step3;
        case 'Step4':
          return Step4;
        case 'Step5':
          return Step5;
        case 'StepConfirmation':
          return StepConfirmation;
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
    updateSteps(option) {
      this.formData.selectedOption = option;

      // Dynamically update step order based on the selected option
      switch (option) {
        case 'option1': // Nouvel arrivant
          this.steps = ['Step1', 'Step2', 'Step3', 'Step4', 'StepConfirmation'];
          break;
        case 'option2': // Modification
          this.steps = ['Step1', 'Step2', 'StepConfirmation'];
          break;
        case 'option3': // Départ
          this.steps = ['Step1', 'Step5', 'StepConfirmation'];
          break;
        case 'option4': // Prolongation uniquement
          this.steps = ['Step1', 'StepConfirmation'];
          break;
        default:
          this.steps = ['Step1', 'Step2', 'Step3', 'Step4', 'StepConfirmation'];
      }

      this.currentStep = 1; // Reset to the first step after changing the order
    },
  },
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
  max-width: 80%;
  background: white;
  padding: 20px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  transition: transform 0.3s ease;
  border: 1px solid var(--lightgristrigo);
}
</style>
