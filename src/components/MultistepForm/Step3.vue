<template>
  <div class="step-software-hardware">
    <h2>Selection des logiciels et du matériel</h2>
    <form @submit.prevent="submitSelections">
      <!-- Software Selection -->
      <div class="software-section">
        <h3>Logiciels</h3>
        <div
          v-for="software in softwareOptions"
          :key="software.name"
          class="software-item"
        >
          <label>
            <input
              type="checkbox"
              :value="software.name"
              v-model="selectedSoftware"
            />
            {{ software.name }}
          </label>

          <!-- Rights Management -->
          <div v-if="selectedSoftware.includes(software.name)" class="rights-management">
            <label for="rights-{{ software.name }}">Droit sur le logiciel :</label>
            <select
              :id="'rights-' + software.name"
              v-model="softwareRights[software.name]"
              required>
              <option value="User" selected>Utilisateur</option>
              <option value="Chef">Chef</option>
              <option value="Admin">Admin</option>
            </select >
          </div>
        </div>
      </div>

      <!-- Hardware Selection -->
      <div class="hardware-section">
        <h3>Matériel</h3>
        <div
          v-for="hardware in hardwareOptions"
          :key="hardware"
          class="hardware-item"
        >
          <label>
            <input
              type="checkbox"
              :value="hardware"
              v-model="selectedHardware"
            />
            {{ hardware }}
          </label>
        </div>
      </div>

      <div class="button-group">
        <button @click="$emit('prev')" class="prev-button" type="button">Précédent</button>
        <button class="next-button" type="submit">Suivant</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: ['formData'],
  data() {
    return {
      softwareOptions: [
        { name: 'Microsoft Office' },
        { name: 'Photoshop' },
        { name: 'AutoCAD' },
        { name: 'SAP' },
      ],
      hardwareOptions: [
        'Laptop',
        'Desktop',
        'Printer',
        'Monitor',
        'Keyboard',
      ],
      selectedSoftware: this.formData.selectedSoftware || [],
      softwareRights: this.formData.softwareRights || {},
      selectedHardware: this.formData.selectedHardware || [],
    };
  },
  methods: {
    submitSelections() {
      this.$emit('next', {
        ...this.formData,
        selectedSoftware: this.selectedSoftware,
        softwareRights: this.softwareRights,
        selectedHardware: this.selectedHardware,
      });
    },
  },
};
</script>

<style scoped>
.step-software-hardware {
  display: flex;
  flex-direction: column;
  align-items: center;
}

h2 {
  color: var(--bleutrigo);
  margin-bottom: 20px;
}

form {
  width: 100%;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.software-section,
.hardware-section {
  background-color: var(--lightgristrigo);
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.software-item,
.hardware-item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.rights-management {
  margin-left: 20px;
}

label {
  font-weight: bold;
}

select {
  margin-left: 10px;
  padding: 5px;
  border: 1px solid var(--lightgristrigo);
  border-radius: 4px;
}

.button-group {
  display: flex;
  justify-content: space-between;
}

.prev-button,
.next-button {
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
