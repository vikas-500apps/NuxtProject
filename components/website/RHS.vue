<template>
  <div>
    <div v-if="website">
      <h2>{{ website.name }}</h2>
      <p>{{ website.url }}</p>
    </div>

    <div v-else>please wait</div>
  </div>
</template>

<script setup lang="ts">
const { $bus } = useNuxtApp();

const website: any = ref({});

onMounted(() => {
  $bus.$on("selected", (data: any) => {
    website.value = data;
  });
});
</script>
