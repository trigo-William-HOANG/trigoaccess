<template>
  <div class="step-button-choice">
    <h2>Motif de la demande</h2>
    <div class="card-group">
      <div
        v-for="option in options"
        :key="option.value"
        :class="['card', { selected: selectedOption === option.value }]"
        @click="selectOption(option.value)"
      >
        <h3>{{ option.label }}</h3>
        <p>{{ option.description }}</p>
      </div>
    </div>

    <!-- Form dynamically rendered -->
    <div v-if="selectedOption" class="form-container">
      <h2>Informations collaborateur</h2>
      <form @submit.prevent="nextStep">
        <!-- Common Fields -->
        <div>
          <label for="name">Nom du collaborateur:</label>
          <input
            id="name"
            v-model="formData.name"
            type="text"
            placeholder="Entrer name"
          />
        </div>

        <div>
          <label for="date">Date d'émission de la demande :</label>
          <input id="date" v-model="formData.date" type="date" />
        </div>

        <!-- Conditional Fields -->
        <div v-if="selectedOption === 'option1' || selectedOption === 'option2' || selectedOption === 'option4'">
          <label for="contrat">Type de contrat: </label>
          <input
            id="contrat"
            v-model="formData.contrat"
            type="text"
            placeholder="Entrer type of contract"
          />
        </div>

        <div v-if="selectedOption === 'option2'">
          <label for="modif">Que voulez-vous modifier ? : </label>
          <input
            id="modif"
            v-model="formData.modif"
            type="text"
            placeholder="Entrer la modification à effectuer"
          />
        </div>

        <div>
          <label for="remarks">Remarque:</label>
          <textarea
            id="remarks"
            v-model="formData.remark"
            placeholder="Entrer remarks"
            rows="3"
          ></textarea>
        </div>

        <div class="button-group">
          <br />
          <button class="next-button" type="submit">Suivant</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: ['formData'],
  data() {
    return {
      options: [
        { label: 'Nouvel arrivant', value: 'option1', description: 'Indication ou emoticone à mettre ici' },
        { label: 'Modification', value: 'option2', description: 'Indication ou emoticone à mettre ici' },
        { label: 'Départ', value: 'option3', description: 'Indication ou emoticone à mettre ici' },
        { label: 'Prolongation uniquement', value: 'option4', description: 'Indication ou emoticone à mettre ici' },
      ],
      selectedOption: this.formData.selectedOption || null,
    };
  },
  methods: {
    selectOption(optionValue) {
      this.selectedOption = optionValue;
      this.formData.selectedOption = optionValue;
      this.$emit('updateSteps', optionValue); // Notify parent of the selected option
    },
    nextStep() {
      this.$emit('next', { ...this.formData });
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
  margin-bottom: 20px;
}

.card-group {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
}

.card {
  flex: 1;
  padding: 20px;
  border: 1px solid var(--lightgristrigo);
  border-radius: 8px;
  background-color: white;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.card h3 {
  margin: 0 0 10px;
  font-size: 1.2em;
  color: var(--bleutrigo);
}

.card p {
  margin: 0;
  font-size: 0.9em;
  color: var(--darkgristrigo);
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.15);
}

.card.selected {
  border-color: var(--bleutrigo);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
  background-color: var(--lightbleutrigo);
}

.form-container {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

label {
  font-weight: bold;
  display: block;
  margin-bottom: 5px;
}

input,
textarea {
  padding: 10px;
  border: 1px solid var(--lightgristrigo);
  border-radius: 4px;
  width: 100%;
  box-sizing: border-box;
}

textarea {
  resize: none;
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
