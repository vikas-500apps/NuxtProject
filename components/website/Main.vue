<template>
  <div>
    <div class="border flex items-center justify-between px-5 pt-5">
      <div>
        <h2 class="text-lg font-medium text-gray-900">Websites</h2>
        <p class="text-sm text-gray-600">list of websites</p>
      </div>
      <!-- Add button starts here -->
      <button
        type="button"
        class="block bg-white hover:bg-gray-100 focus:bg-gray-100 text-gray-900 font-semibold rounded-md mb-1 px-2 py-2 text-xs border border-gray-300"
        @click="isAdd = true"
      >
        <span
          ><PlusIcon class="flex w-5 h-5 mr-2 shrink-0 text-gray-600" /> Add
        </span>
      </button>
      <!-- Add button starts here -->
    </div>

    <!-- List view starts here -->
    <div class="flex flex-col h-full overflow-hidden">
      <div v-if="pending">Please wait</div>
      <WebsiteList
        v-else
        :websites="websites"
        @selected="editWebsite"
        @delete-site="deleteSite"
      />
    </div>
    <!-- List view ends here -->

    <!-- Add website modal starts here -->
    <div class="flex h-full overflow-auto">
      <TransitionRoot appear :show="isAdd" as="template">
        <Dialog as="div" class="relative z-10">
          <TransitionChild
            as="template"
            enter="duration-300 ease-out"
            enter-from="opacity-0"
            enter-to="opacity-100"
            leave="duration-200 ease-in"
            leave-from="opacity-100"
            leave-to="opacity-0"
          >
            <div class="fixed inset-0 bg-black bg-opacity-25" />
          </TransitionChild>

          <div class="fixed inset-0 overflow-y-auto">
            <div
              class="flex min-h-full items-center justify-center p-4 text-center"
            >
              <TransitionChild
                as="template"
                enter="duration-300 ease-out"
                enter-from="opacity-0 scale-95"
                enter-to="opacity-100 scale-100"
                leave="duration-200 ease-in"
                leave-from="opacity-100 scale-100"
                leave-to="opacity-0 scale-95"
              >
                <DialogPanel
                  class="w-full h-full max-w-md transform overflow bg-white p-6 text-left align-middle shadow-xl transition-all"
                  ><WebsiteAdd
                    @add="add"
                    @close="isAdd = false"
                  ></WebsiteAdd>
                </DialogPanel>
              </TransitionChild>
            </div>
          </div>
        </Dialog>
      </TransitionRoot>
    </div>
    <!-- Add website modal ends here -->

    <!-- Edit website modal starts here -->
    <div class="flex h-full overflow-auto">
      <TransitionRoot appear :show="isEdit" as="template">
        <Dialog as="div" class="relative z-10">
          <TransitionChild
            as="template"
            enter="duration-300 ease-out"
            enter-from="opacity-0"
            enter-to="opacity-100"
            leave="duration-200 ease-in"
            leave-from="opacity-100"
            leave-to="opacity-0"
          >
            <div class="fixed inset-0 bg-black bg-opacity-25" />
          </TransitionChild>

          <div class="fixed inset-0 overflow-y-auto">
            <div
              class="flex min-h-full items-center justify-center p-4 text-center"
            >
              <TransitionChild
                as="template"
                enter="duration-300 ease-out"
                enter-from="opacity-0 scale-95"
                enter-to="opacity-100 scale-100"
                leave="duration-200 ease-in"
                leave-from="opacity-100 scale-100"
                leave-to="opacity-0 scale-95"
              >
                <DialogPanel
                  class="w-full h-full max-w-md transform overflow bg-white p-6 text-left align-middle shadow-xl transition-all"
                  ><WebsiteEdit
                    :website="selectedWebsite"
                    @edit="edit"
                    @close="isEdit = false"
                  ></WebsiteEdit>
                </DialogPanel>
              </TransitionChild>
            </div>
          </div>
        </Dialog>
      </TransitionRoot>
    </div>
    <!-- Edit website modal ends here -->
  </div>
</template>
<script setup lang="ts">
const { $bus } = useNuxtApp();
import { PlusIcon } from "@heroicons/vue/24/solid";
import {
  TransitionRoot,
  TransitionChild,
  Dialog,
  DialogPanel,
  DialogTitle,
} from "@headlessui/vue";

const websites: any = ref([]);
const pending: any = ref(false);
const isAdd: any = ref(false);
const isEdit: any = ref(false);
const selectedWebsite: any = ref({});
const authHeader: any = ref({
  "Content-Type": "application/json",
  Authorization:
    "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiODllMDIyMDU2Nzk0NDFjMmFmNWNhZjkzOTYyNGJjZjciLCJkIjoiMTY4MDA5NCIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyODQ4Njh9.GqnTUIEfrTqS4vabLmWc1QvrD9ueJvZZ88Yqb62UCAA",
});
const baseURL: any = ref("https://v1-orm-lib.mars.hipso.cc/websites/");

onMounted(async () => {
  // Get websites
  await get();
});

const editWebsite = (website: any) => {
  selectedWebsite.value = website;
  isEdit.value = true;
};

const edit = async (website: any) => {
  // Put call hits when click on save button
  var response = await $fetch(`${baseURL.value}${website.uid}`, {
    method: "PUT",
    headers: authHeader.value,
    body: website,
  });

  if (response) {
    var websiteIndex: any = ref(null);
    websiteIndex.value = websites.value.findIndex(
      (site: any) => site.name == website.name
    );

    isEdit.value = false;
    websites.value[websiteIndex.value] = website;
  }
};

// Delete website based on uid   uid: string
const deleteSite = async (website: Object) => {
  // Put call hits when click on save button
  var response = await $fetch(`${baseURL.value}${website.uid}`, {
    method: "DELETE",
    headers: authHeader.value,
  });

  if (response) {
    var index: any = ref(null);
    index.value = websites.value.findIndex(
      (site: any) => site.name == website.name
    );
    if (index !== -1) websites.value.splice(index.value, 1);
  }
};

// Post website data
const add = async (form: any) => {
  console.log("add site in main", form);
  const website = await $fetch(`${baseURL.value}`, {
    method: "POST",
    headers: authHeader.value,
    body: form,
  });
  if (website) {
    isAdd.value = false;
    websites.value.push(website);
  }
};

// Get websites data
const get = async () => {
  pending.value = true;
  websites.value = await $fetch(
    "https://v1-orm-lib.mars.hipso.cc/websites/?offset=0&limit=5&sort_column=id&sort_direction=desc",
    {
      method: "GET",
      headers: authHeader.value,
    }
  );
  if (websites.value) {
    pending.value = false;
    if (websites.value.length) $bus.$emit("selected", websites.value[0]);
  }
};
</script>
