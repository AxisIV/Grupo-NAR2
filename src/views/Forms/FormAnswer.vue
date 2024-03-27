<template>
  <div class="row card">
    <div class="col-md-3">
      <img
        src="https://www.mlsvallarta.com/wp-content/uploads/mls-images/13198/GrupoNAR_Logotipo_2Rojo-340x340-c.jpg"
        style="height: 70px; width: 70px; margin-left: 50px"
      />
    </div>
  </div>
  <div class="container p-6 mt-10">
    <div class="progress">
      <div
        class="progress-bar"
        role="progressbar"
        :style="porcentajeProgreso"
        aria-valuenow="25"
        aria-valuemin="0"
        aria-valuemax="100"
      >
        <span class="fs-4">
          {{ ((PreguntaActual + 1) / Preguntas.length) * 100 }}%</span
        >
      </div>
    </div>
    <div v-if="Preguntas.length > 0">
      <div class="pregunta">
        <span>{{ Preguntas[PreguntaActual].Pregunta }}</span>
      </div>
      <div class="form-group mt-5" v-if="getQuestionType() === 'text'">
        <input
          type="text"
          class="form-control"
          placeholder="Escribe tu respuesta"
        />
      </div>
      <div class="form-group mt-5" v-else-if="getQuestionType() === 'radio'">
        <div
          class="custom-control custom-radio custom-control-inline"
          v-for="(option, index) in getQuestionOptions()"
          :key="index"
        >
          <label>
            <input type="radio" class="option-input radio" name="example" />
            Radio option
          </label>
          <label :for="'option' + index">{{ option.valor }}</label>
        </div>
      </div>
      <div class="d-flex justify-content-center mt-12">
        <button
          class="btn btn-primary"
          @click="nextQuestion"
          v-if="PreguntaActual !== Preguntas.length - 1"
        >
          Siguiente
        </button>
        <button
          class="btn btn-success"
          @click="finish"
          v-if="PreguntaActual == Preguntas.length"
        >
          Finalizar
        </button>
        <button
          class="btn btn-dark"
          @click="previousQuestion"
          v-if="PreguntaActual != 0"
        >
          Atras
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { apiApp } from "@/core/api/apiApp";
import { defineComponent, onMounted, ref, computed } from "vue";

interface Question {
  idPregunta: number;
  Pregunta: string;
  idCuestionario: number;
  Tipo: string;
}

export default defineComponent({
  name: "FormAnswer",
  setup() {
    const Preguntas = ref<Question[]>([]);
    const PreguntaActual = ref(0);
    const respuestas = ref([]);

    onMounted(async () => {
      console.log("FormAnswer");
      await apiApp.get("Preguntas/51").then((response) => {
        Preguntas.value = response.data;
        console.log(Preguntas.value);
      });
    });

    const getQuestionType = () => {
      const tipo = JSON.parse(Preguntas.value[PreguntaActual.value].Tipo);
      return tipo.tipo;
    };

    const getQuestionOptions = () => {
      const tipo = JSON.parse(Preguntas.value[PreguntaActual.value].Tipo);
      return tipo.opciones;
    };

    const nextQuestion = () => {
      if (PreguntaActual.value < Preguntas.value.length - 1) {
        PreguntaActual.value++;
      }
    };

    const previousQuestion = () => {
      if (PreguntaActual.value > 0) {
        PreguntaActual.value--;
      }
    };

    const start = () => {
      PreguntaActual.value = 0;
    };

    const finish = () => {
      console.log("Finish");
    };

    const porcentajeProgreso = computed(() => {
      return `width: ${
        ((PreguntaActual.value + 1) / Preguntas.value.length) * 100
      }%`;
    });

    return {
      Preguntas,
      porcentajeProgreso,
      PreguntaActual,
      getQuestionType,
      getQuestionOptions,
      nextQuestion,
      previousQuestion,
      start,
      finish,
    };
  },
});
</script>

<style scoped>
.pregunta {
  font-size: 2rem;
  font-weight: 500;
  color: rgb(72, 72, 72);
  margin-top: 5rem;
  text-align: center;
}

.radioInput {
  width: 30px;
  height: 30px;
  margin-right: 10px;
}
</style>
