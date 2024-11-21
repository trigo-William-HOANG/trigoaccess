<template>
  <div class="step-software-hardware">
    <h2>Sélection du matériel</h2>
    <form @submit.prevent="submitSelections">
      <!-- Hardware Selection -->
      <div class="hardware-section">
        <h3>Matériel</h3>
        <div v-for="hardware in hardwareOptions" :key="hardware.name"
          :class="['hardware-item', { selected: selectedHardware.includes(hardware.name) }]"
          @click="toggleHardwareSelection(hardware.name)">
          <div class="hardware-header">
            {{ hardware.name }}
          </div>

          <!-- Status Selection -->
          <div v-if="selectedHardware.includes(hardware.name) && hardware.status && hardware.status.length"
            class="siteapp-management" @click.stop>
            <label for="status-{{ hardware.name }}">Nature du besoin :</label>

            <fieldset>
              <div v-for="status in hardware.status" :key="status">
                <input type="radio" :id="'status-' + hardware.name + '-' + status" :value="status"
                  v-model="hardwareStatus[hardware.name]" />
                <label :for="'status-' + hardware.name + '-' + status">
                  {{ status }}
                  {{ console.log(hardwareStatus[hardware.name]) }}
                </label>
              </div>
            </fieldset>
          </div>

          <!-- Details Section -->
          <div
            v-if="selectedHardware.includes(hardware.name) && hardwareStatus[hardware.name] === `Réattribution d'un PC du service`"
            class="siteapp-management" @click.stop>
            <div>
              <label :for="'NumPC-' + hardware.name">N° du PC :</label>
              <input type="text" :id="'NumPC-' + hardware.name" v-model="NumeroPC" />
            </div>
            <div>
              <label :for="'NumTeamviewer-' + hardware.name">N° TeamViewer :</label>
              <input type="text" :id="'NumTeamviewer-' + hardware.name" v-model="NumTeamViewer" />
            </div>
          </div>
        </div>
      </div>

      <!-- Submit Buttons -->
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
      hardwareOptions: [
        {
          name: 'PC',
          status: [`Réattribution d'un PC du service`, `Achat d'un nouveau PC`],
        },
      ],
      selectedHardware: this.formData.selectedHardware || [],
      hardwareStatus: this.formData.hardwareStatus || {},
      hardwareDetails: this.formData.hardwareDetails || {},
    };
  },
  methods: {
    toggleHardwareSelection(hardwareName) {
      if (this.selectedHardware.includes(hardwareName)) {
        // Remove hardware from selection
        this.selectedHardware = this.selectedHardware.filter(name => name !== hardwareName);
        delete this.hardwareStatus[hardwareName];
        delete this.hardwareDetails[hardwareName];
      } else {
        // Add hardware to selection
        this.selectedHardware.push(hardwareName);
        this.$set(this.hardwareStatus, hardwareName, null); // Initialize status
        this.$set(this.hardwareDetails, hardwareName, { NumeroPC: '', NumTeamViewer: '' }); // Initialize details
      }
    },
    submitSelections() {
      this.$emit('next', {
        ...this.formData,
        selectedHardware: this.selectedHardware,
        hardwareStatus: this.hardwareStatus,
        hardwareDetails: this.hardwareDetails,
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

.hardware-section {
  background-color: var(--lightgristrigo);
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.hardware-item {
  display: flex;
  flex-direction: column;
  padding: 15px;
  border: 1px solid var(--lightgristrigo);
  border-radius: 4px;
  margin-bottom: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
  background-color: white;
}

.hardware-item.selected {
  border-color: var(--verttrigo);
  background-color: var(--lightbleutrigo);
  transform: scale(1.02);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
}

.hardware-header {
  font-weight: bold;
}

.siteapp-management {
  margin-top: 10px;
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
