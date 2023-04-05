<template>
  <div
    class="fixed inset-0 bg-gray-100 bg-opacity-10 justify-center items-center flex"
  >
    <div class="relative w-full h-full max-w-xl md:h-auto">
      <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
        <div class="p-8">
          <!-- Header starts here -->
          <div class="sm:flex sm:items-start mb-6">
            <div class="text-center sm:mt-0 sm:ml-4 sm:text-left">
              <h3 class="text-base mb-1 font-semibold leading-6 text-gray-900">
                Edit Website
              </h3>
            </div>
          </div>
          <!-- Header ends here -->
          <div>
            <label
              for="email"
              class="block text-sm font-medium leading-6 text-gray-900"
              >Name</label
            >
            <!-- Name field starts here -->
            <div class="mt-2">
              <input
                ref="text_input_ref"
                v-model="name"
                type="textarea"
                oninvalid="Enter at least 5 characters"
                aria-describedby="input-live-help input-url-feedback"
                autofocus
                placeholder="Enter name"
                class="block w-full rounded-md border-0 text-gray-900 shadow-sm p-[8px] ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 focus-visible:ring-2 sm:text-sm sm:leading-6 focus-within:outline-none focus-visible:outline-none focus-visible:shadow-none focus-visible:ring-indigo-600"
              />
            </div>
            <!-- Name field ends here -->
            <!-- URL field starts here -->
            <div class="mt-2">
              <input
                ref="text_input_ref"
                v-model="url"
                type="textarea"
                oninvalid="Enter at least 5 characters"
                aria-describedby="input-live-help input-url-feedback"
                autofocus
                placeholder="Enter url"
                class="block w-full rounded-md border-0 text-gray-900 shadow-sm p-[8px] ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 focus-visible:ring-2 sm:text-sm sm:leading-6 focus-within:outline-none focus-visible:outline-none focus-visible:shadow-none focus-visible:ring-indigo-600"
              />
            </div>
            <!-- URL field ends here -->
          </div>
        </div>
        <!-- Save and cancel buttons starts here -->
        <div
          class="bg-gray-50 p-6 sm:flex sm:flex-row-reverse sm:px-6 rounded-b-md"
        >
          <button
            type="button"
            data-modal-hide="staticModal"
            class="inline-flex w-full justify-center rounded-md bg-gray-900 px-4 py-2 text-sm font-semibold text-white shadow-sm hover:bg-gray-700 sm:ml-3 sm:w-auto"
            @click.prevent="save"
          >
            Save
          </button>
          <button
            type="button"
            class="rounded-md inline-flex w-full justify-center bg-white px-3 py-2 text-sm font-semibold text-gray-900 ring-1 ring-inset ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto"
            @click="emit('close')"
          >
            Cancel
          </button>
        </div>
        <!-- Save and cancel buttons ends here -->
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
const props = defineProps<{ website: Object }>();
const emit = defineEmits(["edit"]);

const name = ref(props.website.name);
const url = ref(props.website.url);
const uid = ref(props.website.uid);

const save = () => {
  emit("edit", { name: name.value, url: url.value, uid: uid.value });
};
</script>
