<template>
  <v-container>
    <v-card-title class="text-center">Registro de Denuncia</v-card-title>

    <v-form @submit.prevent="submitForm" ref="reportFormRef">
      <v-file-input
        v-model="reportPhoto"
        label="Foto del Reporte"
        accept="image/*"
        show-size
        :rules="photoRules"
      ></v-file-input>

      <v-textarea
        v-model="description"
        label="Descripción"
        :rules="descriptionRules"
      ></v-textarea>

      <div class="mt-5 text-center">
        <v-btn type="submit" color="primary">Enviar</v-btn>
      </div>
    </v-form>
  </v-container>
</template>

<script setup>
import { ref } from "vue";

const reportPhoto = ref(null);
const description = ref("");
const reportFormRef = ref(null);

const photoRules = [
  (value) => {
    if (!description.value) {
      return !!value || "La foto del reporte es requerida";
    }
    return true;
  },
];

const descriptionRules = [
  (value) => {
    if (!reportPhoto.value) {
      return !!value || "La descripción es requerida";
    }
    return true;
  },
  (value) => value.length <= 250 || "La longitud máxima es de 250 caracteres",
  (value) =>
    /^[a-zA-Z0-9\s.,?!]+$/.test(value) ||
    "Solo se permiten caracteres alfanuméricos, puntuación y espacios",
];

async function submitForm() {
  const isValid = await reportFormRef.value.validate();

  if (isValid) {
    if (!reportPhoto.value && !description.value) {
      // Both fields are empty, show an error or prevent form submission
      console.log("Error: Both fields are empty");
      return;
    }

    // Handle form submission
    console.log("Report Photo:", reportPhoto.value);
    console.log("Description:", description.value);
    // You can perform further actions like uploading the photo or saving the data
  }
}
</script>
