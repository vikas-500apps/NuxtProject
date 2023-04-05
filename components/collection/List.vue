<template>
  <div v-for="(template, index) in templates" :key="index">
    <!-- Start of templates list-->

    <div @click="emitSelectedTemplate(template)" class="cursor-pointer">
      <!-- DocumentIcon -->
      <span>
        <DocumentIcon
          v-if="template.type"
          class="h-5 w-5 font-[500] text-gray-600 mr-2 sm:mt-[20px]"
        />
      </span>
      <!-- End of icon -->
      <div class="ml-[50px] mt-[-20px] mb-[90px]">
        <p class="text-b">
          {{ template.name }}
        </p>
        <p class="text-gray-600">{{ template.subject }}</p>
      </div>
    </div>
    <!-- End of templates list -->

    <!-- TrashIcon -->
    <span>
      <TrashIcon
        class="h-5 w-5 text-gray-600 ml-[19rem] mt-[-120px]"
        @click="() => emit('delete', template.uid)"
      />
    </span>
    <!-- End of delete icon -->
  </div>
</template>

<script setup lang="ts">
import { DocumentIcon, TrashIcon } from "@heroicons/vue/24/outline";

defineProps({
  templates: {
    type: Array,
    default: () => {
      return [];
    },
  },
});

// Define emits
const emit = defineEmits(["select", "delete"]);

// Emit selected template when click on template
const emitSelectedTemplate = (template: object) => {
  emit("select", template);
};
</script>
<style scoped></style>
