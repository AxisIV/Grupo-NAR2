<template>
  <!--begin::Formulario Widget-->
  <div class="container">
    <div class="row">
      <!-- Sidebar -->
      <div class="col-lg-3">
        <div class="card mb-3">
          <div class="card-header">
            <h4 class="card-title mb-0">Formularios Guardados</h4>
          </div>
          <ul class="list-group list-group-flush">
            <li
              v-for="(savedForm, index) in savedForms"
              :key="index"
              class="list-group-item d-flex justify-content-between align-items-center"
            >
              <span>{{ savedForm.Nombre }}</span>
              <div>
                <button
                  @click="loadSavedForm(savedForm)"
                  class="btn btn-primary btn-sm me-2"
                >
                  <i class="bi bi-cloud-download"></i>
                  <!-- Icono de descarga -->
                </button>
                <button
                  @click="removeSavedForm(index)"
                  class="btn btn-danger btn-sm"
                >
                  <i class="bi bi-trash"></i>
                  <!-- Icono de eliminar -->
                </button>
              </div>
            </li>
          </ul>
          <div class="py-3 text-center">
            <button type="submit" class="btn btn-primary">
              Nuevo Formulario <i class="bi bi-file-earmark-plus"></i>
            </button>
          </div>
        </div>
      </div>
      <!-- /Sidebar -->
      <div class="col-lg-9">
        <div class="card">
          <div class="card-body py-3">
            <form @submit.prevent="submitForm">
              <div class="mb-3">
                <input
                  type="text"
                  v-model="formTitle"
                  class="form-control form-title"
                  placeholder="Título del formulario"
                />
                <textarea
                  v-model="formDesc"
                  class="form-control form-title"
                  placeholder="Descripción del formulario"
                ></textarea>
              </div>
              <div
                v-for="(question, index) in questions"
                :key="index"
                class="mb-3"
              >
                <div class="row">
                  <div class="col-md-8 col-12">
                    <label :for="'question-' + index" class="form-label"
                      >Pregunta {{ index + 1 }}:</label
                    >
                    <input
                      type="text"
                      v-model="question.pregunta"
                      class="form-control form-control-lg form-control-solid mb-3"
                      placeholder="Ingrese una pregunta"
                    />
                  </div>
                  <div class="col-md-3">
                    <div>
                      <label for="tipoPregunta" class="form-label"
                        >Tipo de Pregunta</label
                      >
                      <select
                        v-model="questions[index].tipo"
                        @change="updateQuestionType(index, $event.target.value)"
                        class="form-control form-select"
                      >
                        <option value="text">Texto</option>
                        <option value="radio">Opción múltiple</option>
                      </select>
                    </div>
                  </div>
                  <div class="col-md-1">
                    <div class="mt-7">
                      <button
                        type="button"
                        @click="removeQuestion(index)"
                        class="btn btn-remove"
                      >
                        <i class="bi bi-trash fs-1 fw-bold"></i>
                      </button>
                    </div>
                  </div>
                </div>
                <div class="row">
                  <div v-if="question.tipo === 'radio' && question.opciones">
                    <div
                      v-for="(opcion, opcIndex) in question.opciones"
                      :key="opcIndex"
                    >
                      <div
                        class="input-group"
                        style="margin-top: 12px; margin-bottom: 12px"
                      >
                        <input
                          type="text"
                          :id="'opcion-' + index + '-' + opcIndex"
                          :value="opcion.valor"
                          v-model="question.opciones[opcIndex].valor"
                          class="form-control"
                          placeholder="Opción"
                        />
                        <button
                          type="button"
                          @click="removeOption(index, opcIndex)"
                          class="btn btn-outline-danger"
                        >
                          <i class="bi bi-x fs-2 fw-700"></i>
                        </button>
                      </div>
                    </div>
                    <span
                      @click="addOption(index)"
                      style="cursor: pointer; padding: 12px"
                    >
                      <i
                        style="font-size: 1.5rem"
                        class="bi bi-plus-circle"
                      ></i>
                      <span class="text-muted fs-3">Agregar Opcion</span>
                    </span>
                  </div>
                </div>
              </div>
              <button
                type="button"
                @click="addQuestion"
                class="btn btn-primary me-2"
              >
                Agregar Pregunta
                <i class="bi bi-plus-lg"></i>
              </button>
              <button @click="saveForm" class="btn btn-primary ms-2">
                Guardar Formulario
                <i class="bi bi-floppy"></i>
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent, onMounted } from "vue";
import { apiApp } from "@/core/api/apiApp";
import { useRoute } from "vue-router";

export interface questions {
  tipo: string;
  pregunta: string;
  opciones?: { nombre: string; valor: string }[]; // Arreglo de objetos con nombre y valor
}
export interface SavedForm {
  idCuestionario: number;
  Nombre: string;
  Descripcion: string;
  Preguntas: questions[];
}

export default defineComponent({
  setup() {
    const route = useRoute();
    const formId = ref();
    const questions = ref<questions[]>([]);
    const formTitle = ref<string>("");
    const formDesc = ref<string>("");
    const savedForms = ref<SavedForm[]>([]);
    const selectedType = ref<string>("text");

    const addQuestion = () => {
      let newQuestion: questions = {
        tipo: selectedType.value,
        pregunta: "",
        opciones: [],
      };
      if (selectedType.value === "radio") {
        newQuestion.opciones = [];
      }
      questions.value.push(newQuestion);
    };

    const removeQuestion = (index: number) => {
      questions.value.splice(index, 1);
    };

    const addOption = (index: number) => {
      questions.value[index].opciones.push({ nombre: "", valor: "" });
    };

    const removeOption = (qIndex: number, oIndex: number) => {
      questions.value[qIndex].opciones.splice(oIndex, 1);
    };

    const updateQuestionType = (index: number, newType: string) => {
      questions.value[index].tipo = newType;
      if (newType === "radio") {
        questions.value[index].opciones = []; // Reiniciar opciones si es pregunta de opción múltiple
      }
    };
    const submitForm = async () => {
      try {
        // Crear el objeto formData para enviar al servidor
        const formData = {
          Nombre: formTitle.value,
          Descripcion: formDesc.value,
        };

        // Enviar formData al servidor usando una petición POST para crear el formulario
        const responseForm = await apiApp.post("Cuestionario/", formData);
        const formId = responseForm.data; // Obtener el ID del formulario creado

        // Ahora asociamos las preguntas al formulario usando el ID del formulario
        const preguntasData = questions.value.map((pregunta) => ({
          Pregunta: pregunta.pregunta,
          Tipo: pregunta.tipo,
          idCuestionario: formId,
        }));

        // Enviar las preguntas asociadas al formulario al servidor usando una petición POST
        await apiApp.post("Pregunta/", preguntasData);

        console.log(preguntasData);
        // Limpiar el formulario después de enviar los datos correctamente
        clearForm();
      } catch (error) {
        console.error("Error al enviar formulario:", error);
      }
    };
    const saveForm = async () => {
      try {
        // Crear el objeto formData para enviar al servidor
        const formData = {
          idCuestionario: formId.value, // ID del formulario que se está actualizando
          Nombre: formTitle.value,
          Descripcion: formDesc.value,
        };
        // Enviar formData al servidor usando una petición PUT para actualizar el formulario
        const response = await apiApp.put(
          `Cuestionario/${formId.value}`,
          formData
        );

        const formPreguntas = {
          Preguntas: questions.value.map((pregunta) => ({
            Pregunta: pregunta.pregunta,
            Tipo: pregunta.tipo,
            opciones: pregunta.opciones, // Si las preguntas tienen opciones, incluirlas también
          })),
        };
        const responseQ = await apiApp.put(
          `Pregunta/${formId.value}`,
          formPreguntas
        );

        console.log("Formulario actualizado correctamente:", response.data);
        console.log("Formulario actualizado correctamente:", responseQ.data);
      } catch (error) {
        console.error("Error al actualizar formulario:", error);
      }
    };

    const clearForm = () => {
      formTitle.value = "";
      formDesc.value = "";
      questions.value = [];
    };
    const loadSavedForm = (savedForm) => {
      formId.value = savedForm.idCuestionario;
      formTitle.value = savedForm.Nombre;
      formDesc.value = savedForm.Descripcion;
      questions.value = [...savedForm.Preguntas];
    };
    const removeSavedForm = async (index: number) => {
      try {
        const formToDelete = savedForms.value[index]; // Obtener el formulario a eliminar
        const idCuestionario = formToDelete.idCuestionario; // Obtener el ID del formulario

        // Enviar una solicitud DELETE al endpoint correspondiente con el ID del formulario
        await apiApp.delete(`Cuestionario/${idCuestionario}`);

        // Si la solicitud DELETE es exitosa, remover el formulario de la lista en el frontend
        savedForms.value.splice(index, 1);

        console.log("Formulario eliminado correctamente");
      } catch (error) {
        console.error("Error al eliminar formulario:", error);
      }
    };

    onMounted(async () => {
      try {
        const response = await apiApp.get("Cuestionarios/");
        savedForms.value = response.data;
      } catch (error) {
        console.error("Error al cargar el formulario guardado:", error);
      }
    });

    return {
      addQuestion,
      removeQuestion,
      submitForm,
      questions,
      formTitle,
      formId,
      formDesc,
      loadSavedForm,
      savedForms,
      saveForm,
      removeSavedForm,
      addOption,
      removeOption,
      updateQuestionType,
    };
  },
});
</script>

<style scoped>
.form-container {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}
.form-select {
  display: inline;
  width: 100%;
  padding: 10px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #fff;
  transition: border-color 0.3s ease-in-out;
}
.form-group {
  margin-bottom: 20px;
  position: relative;
}

.form-label {
  font-weight: bold;
  margin-bottom: 8px;
  display: inline-block;
  color: #333;
}

.form-input {
  width: 100%;
  padding: 12px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  transition: border-color 0.3s;
}

.form-input:focus {
  outline: none;
  border-color: #66afe9;
}

.form-title {
  border: none;
  outline: none;
  background-color: transparent;
  font-size: 20px;
  margin-bottom: 12px;
}

.btn {
  padding: 12px 24px;
  font-size: 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s;
}

.btn-add {
  background-color: #4caf50;
  color: white;
}

.btn-submit {
  background-color: #008cba;
  color: white;
}

.btn-remove {
  color: #dc3545;
  background-color: transparent;
}

.btn-remove:hover {
  color: #fff;
  background-color: #dc3545;
}

.btn:hover {
  opacity: 0.8;
}
</style>
