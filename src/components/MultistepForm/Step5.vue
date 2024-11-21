<template>
  <div class="step-departure">
    <h2>Formulaire de départ</h2>
    <form @submit.prevent="submitSelections">
      <!-- Departure Options -->
      <div class="departure-section">
        <h3>Choisissez vos options</h3>
        <div
          v-for="item in departureData"
          :key="item.name"
          :class="['departure-item', { selected: selecteddeparture.includes(item.name) }]"
          @click="toggledepartureSelection(item.name)"
        >
          <!-- Option Title -->
          <div class="departure-header">{{ item.title }}</div>

          <!-- Additional Details -->
          <div
            v-if="selecteddeparture.includes(item.name)"
            class="siteapp-management"
            @click.stop
          >
            <!-- Date Picker -->
            <div class="date-picker">
              <label :for="'date-' + item.name">Jusqu'au :</label>
              <input
                type="date"
                :id="'date-' + item.name"
                v-model="departureDates[item.name]"
              />
              <p>(Par défaut 3 mois)</p>
            </div>

            <!-- Dropdowns -->
            <div v-if="item.redirectTo?.length">
              <label :for="'redirect-' + item.name">Rediriger vers :</label>
              <select
                :id="'redirect-' + item.name"
                v-model="departureRedirection[item.name]"
              >
                <option v-for="person in item.redirectTo" :key="person" :value="person">
                  {{ person }}
                </option>
              </select>
            </div>

            <div v-if="item.accessTo?.length">
              <label :for="'access-' + item.name">Accès accordé à :</label>
              <select
                :id="'access-' + item.name"
                v-model="departureAccess[item.name]"
              >
                <option v-for="person in item.accessTo" :key="person" :value="person">
                  {{ person }}
                </option>
              </select>
            </div>

            <div v-if="item.sendhistoryTo?.length">
              <label :for="'history-' + item.name">Envoyer historique à :</label>
              <select
                :id="'history-' + item.name"
                v-model="departureHistory[item.name]"
              >
                <option v-for="person in item.sendhistoryTo" :key="person" :value="person">
                  {{ person }}
                </option>
              </select>
            </div>
            <div v-if="item.reassignTo?.length">
              <label :for="'reassign-' + item.name">PC réattribué à :</label>
              <select
                :id="'reassign-' + item.name"
                v-model="departureReassign[item.name]"
              >
                <option v-for="person in item.reassignTo" :key="person" :value="person">
                  {{ person }}
                </option>
              </select>
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
    departureData: [], // Initially empty
    selecteddeparture: this.formData.selecteddeparture || [],
    departureRedirection: this.formData.departureRedirection || {},
    departureHistory: this.formData.departureHistory || {},
    departureAccess: this.formData.departureAccess || {},
    departureReassign: this.formData.departureReassign || {},
    departureDates: {}, // Initialize as empty
  };
},
async mounted() {
  // Simulate asynchronous loading of departureData
  this.departureData = await this.loadDepartureData(); // Replace this with actual data loading logic if necessary
  this.departureDates = this.initializeDefaultDates();
},
  methods: {
    async loadDepartureData() {
    // If departureData comes from an API or is otherwise asynchronous, load it here
    return [
      {
        name: 'redirectMail',
        title: 'Souhaitez-vous rediriger les mails ?',
        redirectTo: ['Dumetz, Agnes', 'Said Alaoui, Aymane', 'Tadembo, Alexandre'],
      },
      {
        name: 'historyMail',
        title: "Souhaitez-vous avoir accès à l'historique des mails ?",
        sendhistoryTo: ['Dumetz, Agnes', 'Said Alaoui, Aymane', 'Tadembo, Alexandre'],
      },
      {
        name: 'accessOneDrive',
        title: 'Souhaitez-vous avoir accès au OneDrive ?',
        accessTo: ['Dumetz, Agnes', 'Said Alaoui, Aymane', 'Tadembo, Alexandre'],
      },
      {
        name: 'reassignPC',
        title: 'Le PC doit-il être réattribué ?',
        reassignTo: ['Dumetz, Agnes', 'Said Alaoui, Aymane', 'Tadembo, Alexandre'],
      },
    ];
  },
    toggledepartureSelection(departureName) {
      if (this.selecteddeparture.includes(departureName)) {
        // Deselect item
        this.selecteddeparture = this.selecteddeparture.filter((name) => name !== departureName);
        this.$delete(this.departureDates, departureName);
        this.$delete(this.departureRedirection, departureName);
        this.$delete(this.departureHistory, departureName);
        this.$delete(this.departureAccess, departureName);
        this.$delete(this.departureReassign, departureName);
      } else {
        // Select item
        this.selecteddeparture.push(departureName);
        this.$set(this.departureDates, departureName, this.defaultDate());
        if (departureName === 'redirectMail') this.$set(this.departureRedirection, departureName, null);
        if (departureName === 'historyMail') this.$set(this.departureHistory, departureName, null);
        if (departureName === 'accessOneDrive') this.$set(this.departureAccess, departureName, null);
        if (departureName ==='reassignPC') this.$set(this.departureReassign, departureName, null);
      }
    },
    submitSelections() {
      this.$emit('next', {
        ...this.formData,
        selecteddeparture: this.selecteddeparture,
        departureRedirection: this.departureRedirection,
        departureHistory: this.departureHistory,
        departureAccess: this.departureAccess,
        departureDates: this.departureDates,
        departureReassign: this.departureReassign,
      });
    },
    initializeDefaultDates() {
      const defaultDates = {};
      this.departureData.forEach((item) => {
        defaultDates[item.name] = this.defaultDate();
      });
      return defaultDates;
    },
    defaultDate() {
      const date = new Date();
      date.setMonth(date.getMonth() + 3);
      return date.toISOString().split('T')[0]; // Format as YYYY-MM-DD
    },
  },
};
</script>
  
  <style scoped>
  .step-departure {
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
  
  .departure-section {
    background-color: var(--lightgristrigo);
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  }
  
  .departure-item {
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
  
  .departure-item.selected {
    border-color: var(--verttrigo);
    background-color: var(--lightbleutrigo);
    transform: scale(1.02);
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
  }
  
  .departure-header {
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
  