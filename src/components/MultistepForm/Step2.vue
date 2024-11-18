<template>
  <div class="step-job-details">
    <h2>Détail du collaborateur</h2>
    <form @submit.prevent="submitDetails">
      <div class="form-field">
        <label for="poste">Poste:</label>
        <input id="poste" v-model="formData.poste" type="text" placeholder="Entrer le poste" required />
      </div>

      <div class="form-field">
        <label for="detailPoste">Détail du poste:</label>
        <textarea
          id="detailPoste"
          v-model="formData.detailPoste"
          placeholder="Entrer le détail du poste"
          rows="3"
        ></textarea>
      </div>


      <div class="form-field">
        <label for="societe">Société Trigo:</label>
        <input id="societe" v-model="formData.societe" type="text" placeholder="Entrer la Société affilié Trigo" />
      </div>

      <div class="form-field">
        <label for="service">Service / Département:</label>
        <input id="service" v-model="formData.service" type="text" placeholder="Entrer le Service / Département" />
      </div>

      <div class="form-field">
        <label for="pays">Pays:</label>
        <select id="pays" v-model="formData.pays">
          <option>France</option>
          <option>Autre</option>
        </select>
      </div>

      <div class="form-field">
        <label for="lecteur">Lecteur réseau:</label>
        <input id="lecteur" v-model="formData.lecteur" type="text" placeholder="Entrer le Lecteur réseau nécessaire" />
      </div>

      <div class="form-field">
        <label for="listesDiffusion">Listes de diffusion:</label>
        <input
          id="listesDiffusion"
          v-model="formData.listesDiffusion"
          type="text"
          placeholder="Entrer la Liste de diffusion"
        />
      </div>

      <div class="form-field">
        <label for="accesIntranet">Accès Intranet:</label>
        <select id="accesIntranet" v-model="formData.accesIntranet">
          <option>Only Users</option>
          <option>Admins</option>
          <option>Guests</option>
        </select>
      </div>

      <div class="form-field">
        <label for="telephoneFixe">Téléphone Fixe:</label>
        <input
          id="telephoneFixe"
          v-model="formData.telephoneFixe"
          type="tel"
          placeholder="Entrer N°Téléphone Fixe"
        />
      </div>

      <div class="form-field">
        <label for="mobile">N°Mobile:</label>
        <input id="mobile" v-model="formData.mobile" type="tel" placeholder="Entrer le N°Mobile" />
      </div>

      <div class="form-field">
        <label for="siteAffectation">Site d'affectation:</label>
        <input
          id="siteAffectation"
          v-model="formData.siteAffectation"
          type="text"
          placeholder="Entrer le site d'affectation"
        />
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
  methods: {
    submitDetails() {
      this.$emit('next', { ...this.formData });
    },
  },
};
</script>

<style scoped>
.step-job-details {
  display: flex;
  flex-direction: column;
  align-items: cEntrer;
}

h2 {
  color: var(--bleutrigo);
  margin-bottom: 20px;
}

form {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-field {
  display: flex;
  flex-direction: column;
}

label {
  font-weight: bold;
  margin-bottom: 5px;
}

input,
textarea,
select {
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
