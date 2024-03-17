<template>
  <KTModalCard
    button-text="Ver más"
    modalId="kt_modal_create_app"
  ></KTModalCard>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import KTModalCard from "@/components/cards/Card.vue";
import { getIllustrationsPath } from "@/core/helpers/assets";

export default defineComponent({
  name: "create-app",
  components: {
    KTModalCard,
  },
  setup() {
    return {
      getIllustrationsPath,
    };
  },
});
</script>
