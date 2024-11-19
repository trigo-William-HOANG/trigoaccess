<template>
  <div class="step-confirmation">
    <h2>Confirmer la demande</h2>
    <div class="review-section">
      <h3>Informations Générales</h3>
      <p><strong>Nom:</strong> {{ formData.name }}</p>
      <p><strong>Type de demande:</strong> {{ formData.selectedOption }}</p>
      <p v-if="formData.contrat"><strong>Type de contrat:</strong> {{ formData.contrat }}</p>
      <p v-if="formData.modif"><strong>Modification souhaitée:</strong> {{ formData.modif }}</p>
      <p v-if="formData.remark"><strong>Remarques:</strong> {{ formData.remark }}</p>

      <h3>Logiciels Sélectionnés</h3>
      <div v-if="formData.selectedSoftware && formData.selectedSoftware.length">
        <ul>
          <li
            v-for="software in formData.selectedSoftware"
            :key="software"
          >
            <strong>{{ software }}</strong> - Droit: {{ formData.softwareRights[software] }}
          </li>
        </ul>
      </div>
      <p v-else>Aucun logiciel sélectionné.</p>

      <h3>Matériel Sélectionné</h3>
      <div v-if="formData.selectedHardware && formData.selectedHardware.length">
        <ul>
          <li
            v-for="hardware in formData.selectedHardware"
            :key="hardware"
          >
            {{ hardware }}
          </li>
        </ul>
      </div>
      <p v-else>Aucun matériel sélectionné.</p>
    </div>

    <div class="button-group">
      <button @click="$emit('prev')" class="prev-button">Précédent</button>
      <button @click="confirmSubmission" class="confirm-button">Confirmer</button>
    </div>

    <div v-if="submitted" class="success-message">
      <p>Votre demande a été transmise avec succès au service correspondant. Merci.</p>
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
      // Simulated form submission
      console.log('Form data:', this.formData);

      // Mark form as submitted
      this.submitted = true;

      // Redirect after a delay
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

h3 {
  color: var(--verttrigo);
  margin-top: 20px;
}

.review-section {
  width: 100%;
  max-width: 600px;
  background-color: var(--lightgristrigo);
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

p,
li {
  color: var(--darkgristrigo);
  margin: 8px 0;
}

ul {
  padding-left: 20px;
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
