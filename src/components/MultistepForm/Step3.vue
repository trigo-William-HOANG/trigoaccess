<template>
  <div class="step-software-hardware">
    <h2>Sélection des logiciels et du matériel</h2>
    <form @submit.prevent="submitSelections">
      <!-- Software Selection -->
      <div class="software-section">
        <div>
          <label for="sites"> Veuillez renseigner les sites à reporter sur tous les logiciels </label>
          <select
          :id="sites"
          v-model="sitesSelection">
            <option v-for="site in sites" :key="site" :value="site">
              {{ site }}
            </option>
          </select>
        </div>
        <br>
        <h3>Logiciels</h3>
        <br />
        <div
          v-for="software in softwareOptions"
          :key="software.name"
          :class="['software-item', { selected: selectedSoftware.includes(software.name) }]"
          @click="toggleSoftwareSelection(software.name)"
        >
          <div class="software-header">
            {{ software.name }}
          </div>

          <!-- Rights Selection -->
          <div
            v-if="selectedSoftware.includes(software.name) && software.rights && software.rights.length"
            class="rights-management"
            @click.stop
          >
            <label for="rights-{{ software.name }}">{{ software.type }} {{ software.name }} demandé :</label>
            <select
              :id="'rights-' + software.name"
              v-model="softwareRights[software.name]"
            >
              <option v-for="right in software.rights" :key="right" :value="right">
                {{ right }}
              </option>
            </select>
          </div>

          <!-- Siteapp Selection -->
          <div
            v-if="selectedSoftware.includes(software.name) && software.siteapp && software.siteapp.length"
            class="siteapp-management"
            @click.stop
          >
            <label for="siteapp-{{ software.name }}">Site d'application :</label>
            <select
              :id="'siteapp-' + software.name"
              v-model="softwareSiteapp[software.name]"
            >
              <option v-for="site in software.siteapp" :key="site" :value="site">
                {{ site }}
              </option>
            </select>
          </div>

          <!-- Siteapp Complémentaire Selection -->
          <div
            v-if="selectedSoftware.includes(software.name) && software.siteappcomp && software.siteappcomp.length"
            class="siteapp-management"
            @click.stop
          >
            <label for="siteappcomp-{{ software.name }}">Site complémentaire :</label>
            <select
              :id="'siteappcomp-' + software.name"
              v-model="softwareSiteapp[software.name]"
            >
              <option v-for="site in software.siteappcomp" :key="site" :value="site">
                {{ site }}
              </option>
            </select>
          </div>

          <!-- Droit temporaire Checkbox -->
          <div
            v-if="selectedSoftware.includes(software.name) && software.name === 'Etraq'"
            class="temporary-rights"
            @click.stop
          >
            <label>
              <input
                type="checkbox"
                v-model="temporaryRights[software.name]"
              />
              Droit temporaire
            </label>
          </div>

          <!-- Signature des commandes Checkbox -->
          <div
            v-if="selectedSoftware.includes(software.name) && software.name === 'Trigo Shop'"
            class="temporary-rights"
            @click.stop
          >
            <label>
              <input
                type="checkbox"
                v-model="temporaryRights[software.name]"
              />
              Signature des commandes
            </label>
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
      sites:[ 'TFRA', 'Autre'],
      softwareOptions: [
        { name: 'Etraq', rights: ['Accès de base (N1)', 'Chef d\'équipe', 'Cash Collector', 'Contrôle de gestion'], type: 'Profil', siteapp: ['TFRA', 'autre'] },
        { name: 'EagleAct', rights: ['Lecture', 'Lecture / Modification', 'Administrateur'], type: 'Profil' },
        { name: 'Audit' },
        { name: 'Qualnet', rights: ['QUALNET SITE MANAGEMENT', 'QUALNET SALES USER'], type: 'Profil', siteapp: ['TFRA', 'Autre'] },
        { name: 'Notilus' },
        { name: 'Peopulse', rights: [`Demande d'intérimaires`, `Administrateur`], type: 'Profil', siteapp: ['TFRA'], siteappcomp: ['Autre'] },
        { name: 'Trigo Shop', siteapp: ['TFRA', 'Autre'] },
      ],
      selectedSoftware: this.formData.selectedSoftware || [],
      softwareRights: this.formData.softwareRights || {},
      softwareSiteapp: this.formData.softwareSiteapp || {},
      temporaryRights: this.formData.temporaryRights || {},
    };
  },
  methods: {
    toggleSoftwareSelection(softwareName) {
      if (this.selectedSoftware.includes(softwareName)) {
        this.selectedSoftware = this.selectedSoftware.filter(name => name !== softwareName);
        delete this.softwareRights[softwareName];
        delete this.softwareSiteapp[softwareName];
        delete this.temporaryRights[softwareName];
      } else {
        this.selectedSoftware.push(softwareName);
        this.$set(this.softwareRights, softwareName, null);
        this.$set(this.softwareSiteapp, softwareName, null);
        this.$set(this.temporaryRights, softwareName, false);
      }
    },
    submitSelections() {
      this.$emit('next', {
        ...this.formData,
        selectedSoftware: this.selectedSoftware,
        softwareRights: this.softwareRights,
        softwareSiteapp: this.softwareSiteapp,
        temporaryRights: this.temporaryRights,
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

.software-section {
  background-color: var(--lightgristrigo);
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.software-item {
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

.software-item.selected {
  border-color: var(--verttrigo);
  background-color: var(--lightbleutrigo);
  transform: scale(1.02);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
}

.software-header {
  font-weight: bold;
}

.rights-management,
.siteapp-management,
.temporary-rights {
  margin-top: 10px;
}

label {
  font-weight: bold;
}

select {
  margin-top: 5px;
  padding: 5px;
  border: 1px solid var(--lightgristrigo);
  border-radius: 4px;
  width: 100%;
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
