<template>
  <div class="row">
    <div class="col-md-8">
      <Editor
        apiKey="ktu1mpe12zfnohqoq3fm63rfq917q3euky0e3x7ch15q7ql1"
        initialValue="<p>Initial editor content</p>"
        :init="{
          height: 500,
          menubar: false,
          plugins: [
            `advlist autolink lists link image charmap`,
            `searchreplace visualblocks code fullscreen`,
            `print preview anchor insertdatetime media`,
            `paste code help wordcount table`,
          ],
          toolbar: `undo redo | formatselect | bold italic | \
          alignleft aligncenter alignright | \
          bullist numlist outdent indent | help`,
        }"
      />
    </div>
    <div class="col-md-4">
      <div class="card">
        <div class="card-header">
          <h3 class="card-title">Opciones</h3>
        </div>
        <div class="card-body">
          <div
            class="card row p-2 fs-4 mb-3"
            draggable="true"
            @dragstart="dragStart($event, ' /Pregunta 1')"
          >
            Pregunta 2
          </div>
          <div class="card row p-2 fs-4 mb-3">Pregunta 2</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Editor from "@tinymce/tinymce-vue";

export default defineComponent({
  name: "editors-tinymce",
  components: {
    Editor,
  },
  setup() {
    const dragStart = (event: DragEvent, pregunta: string) => {
      event.dataTransfer!.setData("text", pregunta);
    };

    return {
      dragStart,
    };
  },
});
</script>
