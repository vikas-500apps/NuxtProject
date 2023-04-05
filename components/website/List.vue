<template>
  <div>
    <div
      v-for="(site, index) in props.websites"
      :key="index"
      class="grow overflow-auto overflow grid-cols-1 divide-y divide-gray-600"
    >
      <div
        class="hover:bg-gray-100 cursor-pointer px-4 py-3"
        @click="selected(site)"
      >
        <h4 class="font-normal text-[15px]">
          {{ site.name }}
        </h4>
        <p class="text-gray-600">{{ site.url }}</p>
        <div class="flex items-center justify-end">
          <PencilSquareIcon
            class="w-[19px] mr-2 text-gray-600 hover:text-indigo-700"
            @click.stop="emit('selected', site)"
          />
          <TrashIcon
            class="w-[19px] mr-2 text-gray-600 hover:text-red-700"
            @click="emit('delete-site', site)"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { PencilSquareIcon, TrashIcon } from "@heroicons/vue/24/outline";
const { $bus } = useNuxtApp();

const props = defineProps<{ websites: String[] }>();
const emit = defineEmits(["selected", "delete-site"]);

const selected = (site: any) => {
  $bus.$emit("selected", site);
};

</script>
<style scoped></style>
