<template>
  <div class="row">
    <div class="col-md-3">
      <img
        src="https://www.mlsvallarta.com/wp-content/uploads/mls-images/13198/GrupoNAR_Logotipo_2Rojo-340x340-c.jpg"
        style="height: 70px; width: 70px; margin-left: 50px"
      />
    </div>
    <div class="col-md-6">
      <span class="titulo">{{ Forms.Nombre }}</span>
    </div>
    <div class="col-md-3">
      <button class="redes"><i class="bi bi-whatsapp"></i></button>
      <button class="redes">
        <i class="bi bi-instagram"></i>
      </button>
      <button class="redes"><i class="bi bi-facebook"></i></button>
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
          v-model="respuestaTexto"
          placeholder="Escribe tu respuesta"
        />
      </div>
      <div class="mt-5 text-center" v-else-if="getQuestionType() === 'radio'">
        <label
          v-for="(option, index) in getQuestionOptions()"
          :key="index"
          class="radio-option"
        >
          <input
            type="radio"
            name="respuestaRadio"
            :value="option.valor"
            v-model="respuestaSeleccionada"
          />
          <span class="circulo"></span>
          <span class="fs-3 py-auto">{{ option.valor }}</span>
        </label>
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
import { useRoute } from "vue-router";

interface Question {
  idPregunta: number;
  Pregunta: string;
  idCuestionario: number;
  Tipo: string;
}
interface Form {
  idCuestionario: number;
  Nombre: string;
  Descripcion: string;
}

interface Respuesta {
  idCliente: number;
  idPregunta: number;
  respuesta: string | number;
}

export default defineComponent({
  name: "FormAnswer",
  setup() {
    const Preguntas = ref<Question[]>([]);
    const Forms = ref<Form[]>([]);
    const PreguntaActual = ref(0);
    const respuestas = ref<Respuesta[]>([]);
    const route = useRoute();
    const respuestaTexto = ref("");
    const respuestaSeleccionada = ref("");

    onMounted(async () => {
      console.log("FormAnswer");
      await apiApp.get("Preguntas/" + route.params.id).then((response) => {
        Preguntas.value = response.data;
        console.log(Preguntas.value);
      });
      await apiApp.get("Cuestionario/51").then((response) => {
        Forms.value = response.data[0];
        console.log(Forms.value);
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

    const nextQuestion = async () => {
      if (PreguntaActual.value < Preguntas.value.length - 1) {
        // Guardar la respuesta actual
        const preguntaActual = Preguntas.value[PreguntaActual.value];
        let respuestaActual: Respuesta;

        if (getQuestionType() === "text") {
          // Respuesta de tipo texto
          respuestaActual = {
            idCliente: 1,
            idPregunta: preguntaActual.idPregunta,
            respuesta: respuestaTexto.value, // Utilizamos la variable del input de texto
          };
        } else if (getQuestionType() === "radio") {
          // Respuesta de opciones de radio
          respuestaActual = {
            idCliente: 1,
            idPregunta: preguntaActual.idPregunta,
            respuesta: respuestaSeleccionada.value, // Utilizamos la variable del input de radio
          };
        }

        try {
          await apiApp.post("Respuesta/", respuestaActual);
          respuestas.value.push(respuestaActual);
        } catch (error) {
          console.error("Error al guardar la respuesta:", error);
        }

        // Pasar a la siguiente pregunta
        PreguntaActual.value++;
      }
    };

    const previousQuestion = async () => {
      // Obtener la respuesta de la pregunta actual
      const preguntaActual = Preguntas.value[PreguntaActual.value];
      const idPreguntaActual = preguntaActual.idPregunta;
      try {
        const response = await apiApp.get(`Respuesta/${idPreguntaActual}`);
        console.log("Respuesta de la pregunta actual:", response.data);
      } catch (error) {
        console.error("Error al obtener la respuesta:", error);
      }

      if (PreguntaActual.value > 0) {
        PreguntaActual.value--;
      }
    };

    const start = () => {
      PreguntaActual.value = 0;
    };

    const finish = async () => {
      console.log("Finish");
      try {
        await apiApp.post("Respuesta/", respuestas.value);
        console.log("Respuestas guardadas exitosamente");
      } catch (error) {
        console.error("Error al guardar las respuestas:", error);
      }
    };

    const porcentajeProgreso = computed(() => {
      return `width: ${
        ((PreguntaActual.value + 1) / Preguntas.value.length) * 100
      }%`;
    });

    return {
      Preguntas,
      Forms,
      porcentajeProgreso,
      PreguntaActual,
      getQuestionType,
      getQuestionOptions,
      nextQuestion,
      previousQuestion,
      start,
      finish,
      respuestaTexto,
      respuestaSeleccionada,
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
.titulo {
  display: flex; /* Establece el contenedor como flexible */
  align-items: center; /* Centra verticalmente */
  height: 70px;
  text-align: center;
  vertical-align: middle;
  font-size: 20px;
}
.redes {
  padding: 10px; /* Espacio interno alrededor del icono */
  border: none; /* Eliminar borde del botón */
  background-color: transparent; /* Fondo transparente */
  cursor: pointer; /* Cambiar cursor al pasar por encima */
  transition: background-color 0.3s ease; /* Transición suave */
}
.redes:hover {
  background-color: #f0f0f0; /* Cambiar color de fondo al pasar el mouse */
}

.bi {
  font-size: 24px; /* Tamaño del ícono */
  vertical-align: middle; /* Alinear verticalmente */
}
.radio-option {
  display: inline-block;
  cursor: pointer;
  font-family: Arial, sans-serif;
  margin-right: 20px; /* Espacio entre los botones */
  margin-top: 5px;
}

/* Ocultamos el input radio original */
.radio-option input[type="radio"] {
  display: none;
}

/* Estilo para simular el botón de radio no seleccionado */
.radio-option .circulo {
  height: 30px;
  width: 30px;
  border-radius: 50%;
  display: inline-block;
  vertical-align: middle;
  border: 2px solid #cccccc;
  margin-right: 10px;
  transition: border 0.3s;
}

/* Estilo para cuando el botón de radio está seleccionado */
.radio-option input[type="radio"]:checked + .circulo {
  border: 6px solid #20c7b1; /* Ajusta el tamaño del borde seleccionado */
}

/* El pseudo-elemento para el signo 'X' */
.radio-option input[type="radio"]:checked + .circulo:before {
  color: white;
  position: absolute;
  font-size: 18px; /* Tamaño del símbolo */
  line-height: 14px; /* Altura de la línea para centrar verticalmente el símbolo */
  margin-left: 5px; /* Ajuste horizontal del símbolo */
  margin-top: 1px; /* Ajuste vertical del símbolo */
}

/* Estilo adicional para cambiar el fondo cuando se pasa el mouse */
.radio-option .circulo:hover {
  background-color: #f1f1f1;
}
</style>
