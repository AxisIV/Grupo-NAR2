<template>
  <!--begin::Card-->
  <div>
    <!--begin::Card body-->
    <div class="p-0">
      <!--begin::Heading-->
      <div class="text-center py-20 my-10">
        <div>
          <!--begin::Title-->
          <h3 class="fs-2x fw-bold mb-10">{{ title }}</h3>
          <!--end::Title-->
        </div>
        <div class="card-actions">
          <!--begin::Action-->
          <button
            type="button"
            class="btn btn-primary btn-sm er fs-6 px-8 py-4"
            data-bs-toggle="modal"
            :data-bs-target="`#${modalId}`"
          >
            {{ buttonText }}
          </button>
          <!--end::Action-->
        </div>

        <!--begin::Description-->
        <p class="text-gray-500 fs-5 fw-semibold mb-13">
          <span v-html="description"></span>
        </p>
        <!--end::Description-->
      </div>
      <!--end::Heading-->

      <!--begin::Illustration-->
      <div class="text-center px-5">
        <img :src="image" alt="" class="mw-100 mh-300px" />
      </div>
      <!--end::Illustration-->
    </div>
    <!--end::Card body-->
  </div>
  <!--end::Card-->
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "modal-card",
  props: {
    title: String,
    description: String,
    buttonText: String,
    image: String,
    modalId: String,
  },
  components: {},
});
</script>
