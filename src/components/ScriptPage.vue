<script setup lang="ts">
import { useRoute, useRouter } from 'vue-router';
import { onMounted, ref } from 'vue';

const snippetContent = ref('');
const snippetSlug = ref('');


const route = useRoute();
const router = useRouter();


const getSnippet = async () => {
    const slug = route.path.substr(1,);
    const response = await fetch(`http://localhost:3000/api/snippets/${slug}`, {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
      },
    });
    if (response.status === 404) {
        router.push('/404')
    }
    const snippetObj = (await response.json());
    snippetContent.value = snippetObj.snippet;
    snippetSlug.value = snippetObj.slug;
}

onMounted(() => {
    getSnippet();
})
</script>

<template>
  <div class="text-center p-20">
    <h1 class="text-4xl font-bold text-gray-800 mb-2">{{snippetSlug}}</h1>
    <div class="flex flex-col space-y-4 p-4">
      <textarea 
        v-model="snippetContent"
        disabled
        class="w-full h-64 p-4 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent"
        placeholder="Write your script here...">
      </textarea>
      <!-- Rest of your code for file upload input -->
    </div>
  </div>
</template>

<style scoped>
</style>
