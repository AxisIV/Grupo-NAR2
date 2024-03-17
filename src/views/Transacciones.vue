<template>
  <div>
    <TableTransacciones />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TableTransacciones from "@/components/widgets/tables/Widget11.vue";

export default defineComponent({
  name: "Transacciones",
  components: {
    TableTransacciones,
  },
  setup() {},
});
</script>
