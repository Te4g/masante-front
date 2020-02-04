<template>
  <div class="calculateurKcal container d-flex flex-column mt-5 w-50">
    <div>
      Votre taille:
      <span v-show="taille"
        >{{ taille }} cm
        <b-button v-model="taille" @click="taille--" variant="info" size="sm"
          >-</b-button
        >
        <b-button v-model="taille" @click="taille++" variant="info" size="sm"
          >+</b-button
        ></span
      >
    </div>
    <input
      id="taille"
      v-model="taille"
      type="range"
      pattern="[0-9]*"
      placeholder="Votre taille (en cm)"
      min="120"
      max="210"
    />

    <input
      v-model="poids"
      type="number"
      pattern="[0-9]*"
      placeholder="Votre poids (en kg)"
      min="40"
      class="mt-2"
    />

    <input
      v-model="age"
      type="number"
      pattern="[0-9]*"
      placeholder="Votre age"
      min="18"
      class="mt-2"
    />

    <label for="sexe" class="mt-2">
      <select id="sexe" v-model="sexe" name="sexe">
        <option value="Quel est votre sexe?" disabled>
          Quel est votre sexe?
        </option>
        <option value="f">Femme</option>
        <option value="h">Homme</option>
      </select>
    </label>

    <label for="niveauActivite" class="">
      <select
        id="niveauActivite"
        v-model="niveauActivite"
        name="niveauActivite"
      >
        <option value="Quel est votre profil?">Quel est votre profil ?</option>
        <option value="sedentaire">Sédentaire</option>
        <option value="actif">Actif</option>
        <option value="sportif">Sportif</option>
      </select>
    </label>
    <ul>
      <li>
        le métabolisme de base est de
        <span class="alert-success">{{ calculMB() }} KCal</span>
      </li>
      <li>le besoin KCal journalier est de {{ calculKCal() }} Kcal</li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {},
  data() {
    return {
      poids: null,
      taille: null,
      age: null,
      sexe: 'Quel est votre sexe?',
      niveauActivite: 'Quel est votre profil?',
      imc() {
        return this.poids / (this.taille / 100) ** 2
      }
    }
  },
  methods: {
    calculMB() {
      if (this.age <= 60 && this.imc() < 25) {
        /* Formule de Harris et Benedict recalculée par Roza et Shizgal (1994) */
        if (this.sexe === 'h') {
          return (
            13.707 * this.poids +
            (492.3 * this.taille) / 100 -
            6.673 * this.age +
            77.607
          ).toFixed(2)
        } else if (this.sexe === 'f') {
          return (
            9.74 * this.poids +
            (172.9 * this.taille) / 100 -
            4.737 * this.age +
            667.051
          ).toFixed(2)
        }
      } else if (this.age > 60 || this.imc() >= 25) {
        /* formule de Black et Al */
        if (this.sexe === 'h') {
          return (
            259 *
            this.poids ** 0.48 *
            (this.taille / 100) ** 0.5 *
            this.age ** -0.13
          ).toFixed(2)
        } else if (this.sexe === 'f') {
          return (
            230 *
            this.poids ** 0.48 *
            (this.taille / 100) ** 0.5 *
            this.age ** -0.13
          ).toFixed(2)
        }
      }
    },
    calculKCal() {
      if (this.niveauActivite === 'sedentaire') {
        return (this.calculMB() * 1.37).toFixed(2)
      } else if (this.niveauActivite === 'actif') {
        return (this.calculMB() * 1.55).toFixed(2)
      } else if (this.niveauActivite === 'sportif') {
        return (this.calculMB() * 1.8).toFixed(2)
      }
    }
  }
}
</script>
