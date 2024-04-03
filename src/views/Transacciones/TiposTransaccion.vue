<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <table class="table align-middle gs-0 gy-4">
          <thead>
            <tr class="fw-bold text-muted bg-light">
              <th class="ps-4 min-w-300px rounded-start">Transaccion</th>
              <th class="min-w-250px">Formulario Fisica</th>
              <th class="min-w-250px">Formulario Moral</th>
              <th class="min-w-200px">Pasos</th>
            </tr>
          </thead>

          <tbody>
            <tr>
              <td>
                <span class="text-gray-900 fw-bold mb-1 fs-6"
                  >Transaccion 1</span
                >
              </td>
              <td>
                <select class="form-select mt-3">
                  <option selected>Formulario</option>
                </select>
              </td>
              <td>
                <select class="form-select mt-3">
                  <option selected>Formulario</option>
                </select>
              </td>
              <td>
                <button class="btn btn-sm btn-primary">Ver Pasos</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted } from "vue";

export default defineComponent({
  name: "TiposTransaccion",

  setup() {
    onMounted(() => {
      apiApp.get("/transacciones").then((response) => {
        console.log(response.data);
      });
    });
    return {};
  },
});
</script>
