<template>
  <Detalles />
  <div class="row">
    <div class="col-md-6">
      <Acciones />
    </div>
    <div class="col-md-6">
      <!--begin::Accordion-->
      <div class="accordion" id="kt_accordion_1">
        <div class="accordion-item">
          <h2 class="accordion-header" id="kt_accordion_1_header_1">
            <button
              class="accordion-button fs-4 fw-semibold"
              type="button"
              data-bs-toggle="collapse"
              data-bs-target="#kt_accordion_1_body_1"
              aria-expanded="true"
              aria-controls="kt_accordion_1_body_1"
            >
              Accordion Item #1
            </button>
          </h2>
          <div
            id="kt_accordion_1_body_1"
            class="accordion-collapse collapse show"
            aria-labelledby="kt_accordion_1_header_1"
            data-bs-parent="#kt_accordion_1"
          >
            <div class="accordion-body">...</div>
          </div>
        </div>

        <div class="accordion-item">
          <h2 class="accordion-header" id="kt_accordion_1_header_2">
            <button
              class="accordion-button fs-4 fw-semibold collapsed"
              type="button"
              data-bs-toggle="collapse"
              data-bs-target="#kt_accordion_1_body_2"
              aria-expanded="false"
              aria-controls="kt_accordion_1_body_2"
            >
              Accordion Item #2
            </button>
          </h2>
          <div
            id="kt_accordion_1_body_2"
            class="accordion-collapse collapse"
            aria-labelledby="kt_accordion_1_header_2"
            data-bs-parent="#kt_accordion_1"
          >
            <div class="accordion-body">
              <!--begin::Item-->
              <div class="d-flex flex-aligns-center pe-10 pe-lg-20">
                <!--begin::Icon-->
                <img
                  alt=""
                  class="w-30px me-3"
                  :src="getAssetPath('media/svg/files/pdf.svg')"
                />
                <!--end::Icon-->

                <!--begin::Info-->
                <div class="ms-1 fw-semibold">
                  <!--begin::Desc-->
                  <a href="#" class="fs-6 text-hover-primary fw-bold"
                    >Finance KPI App Guidelines</a
                  >
                  <!--end::Desc-->

                  <!--begin::Number-->
                  <div class="text-gray-500">1.9mb</div>
                  <!--end::Number-->
                </div>
                <!--end::Info-->

                <!-- Buttons for Accept and Decline -->
                <div class="ms-auto">
                  <button class="btn btn-outline-success btn-sm me-2">
                    Aceptar
                  </button>
                  <button class="btn btn-outline-danger btn-sm">
                    Declinar
                  </button>
                </div>
              </div>
              <!--end::Item-->

              <!--begin::Item-->
              <div class="d-flex flex-aligns-center pe-10 pe-lg-20">
                <!--begin::Icon-->
                <img
                  alt=""
                  class="w-30px me-3"
                  :src="getAssetPath('media/svg/files/doc.svg')"
                />
                <!--end::Icon-->

                <!--begin::Info-->
                <div class="ms-1 fw-semibold">
                  <!--begin::Desc-->
                  <a href="#" class="fs-6 text-hover-primary fw-bold"
                    >Client UAT Testing Results</a
                  >
                  <!--end::Desc-->

                  <!--begin::Number-->
                  <div class="text-gray-500">18kb</div>
                  <!--end::Number-->
                </div>
                <!--end::Info-->

                <!-- Buttons for Accept and Decline -->
                <div class="ms-auto">
                  <button class="btn btn-outline-success btn-sm me-2">
                    Aceptar
                  </button>
                  <button class="btn btn-outline-danger btn-sm">
                    Declinar
                  </button>
                </div>
              </div>
              <!--end::Item-->
            </div>
          </div>
        </div>

        <div class="accordion-item">
          <h2 class="accordion-header" id="kt_accordion_1_header_3">
            <button
              class="accordion-button fs-4 fw-semibold collapsed"
              type="button"
              data-bs-toggle="collapse"
              data-bs-target="#kt_accordion_1_body_3"
              aria-expanded="false"
              aria-controls="kt_accordion_1_body_3"
            >
              Accordion Item #3
            </button>
          </h2>
          <div
            id="kt_accordion_1_body_3"
            class="accordion-collapse collapse"
            aria-labelledby="kt_accordion_1_header_3"
            data-bs-parent="#kt_accordion_1"
          >
            <div class="accordion-body">...</div>
          </div>
        </div>
      </div>
      <!--end::Accordion-->
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { getAssetPath } from "@/core/helpers/assets";
import Detalles from "@/components/page-layouts/Profile.vue";
import Acciones from "@/components/widgets/lists/Widget5.vue";

export default defineComponent({
  components: {
    Detalles,
    Acciones,
  },
  setup() {
    return {
      getAssetPath,
    };
  },
});
</script>
