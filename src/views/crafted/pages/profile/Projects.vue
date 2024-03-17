<template>
  <!--begin::Toolbar-->
  <div class="d-flex flex-wrap flex-stack my-5">
    <!--begin::Heading-->
    <h2 class="fs-2 fw-semibold my-2">Tipos</h2>
    <!--end::Heading-->
  </div>
  <!--end::Toolbar-->

  <!--begin::Row-->
  <div class="row g-6 g-xl-9">
    <!--begin::Col-->
    <div class="col-md-6 col-xl-4">
      <KTCard
        status-data-badge-color="badge-light"
        title="Transaccion 1"
        date="Moral"
        budget="Fisica"
      >
      </KTCard>
    </div>
    <!--end::Col-->

    <!--begin::Col-->
    <div class="col-md-6 col-xl-4">
      <KTCard
        status-data-badge-color="badge-light"
        title="Transaccion 2"
        date="Moral"
        budget="Fisica"
      >
      </KTCard>
    </div>
    <!--end::Col-->

    <!--begin::Col-->
    <div class="col-md-6 col-xl-4">
      <KTCard
        status-data-badge-color="badge-light-success"
        title="Transaccion 3"
        date="Moral"
        budget="Fisica"
      >
      </KTCard>
    </div>
    <!--end::Col-->
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import KTCard from "@/components/cards/Card1.vue";
import KTModalCard from "@/components/cards/Card.vue";

export default defineComponent({
  name: "profile-projects",
  components: {
    KTCard,
    KTModalCard,
  },
});
</script>
