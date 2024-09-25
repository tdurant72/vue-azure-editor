<template>
    <div class="container mx-auto max-w-4xl my-8">
        <form @submit.prevent="submitForm" class="space-y-8">
            <div>
                <label class="block text-gray-800 font-bold mt-4 mb-2" for="title">Title:</label>
                <input
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="title" v-model="formData.title" required>
            </div>
            <div>
                <label class="hidden text-gray-800 font-bold mt-4 mb-2" for="slug">Slug:</label>
                <input
                    class="hidden shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="slug" v-model="slugComputed" readonly>
            </div>
             <div>
      <label  class="block text-gray-800 font-bold mt-4 mb-2" for="postImage">Post Image:</label>
      <input  class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"  id="postImage" type="file" @change="handleImageUpload" accept="image/*">
    </div>
    <div>
      <label  class="block text-gray-800 font-bold mt-4 mb-2" for="createDate">Create Date:</label>
      <input  class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"  id="createDate" type="date" v-model="formData.createDate" required>
    </div>
            <Editor v-model="formData.content" />
            <button class="hover:shadow-form w-full rounded-md bg-[#6A64F1] py-3 px-8 text-center text-base font-semibold text-white outline-none" type="submit">Submit</button>
        </form>
    </div>
</template>

<script setup>
import { ref, computed, } from 'vue';
import Editor from './Editor.vue';

const API_URL = import.meta.env.VITE_API_URL;
const handleImageUpload = (event) => {
      const file = event.target.files[0];
      if (file) {
        formData.value.postImage = file;
      }
    };
const slugComputed = computed(() => {
    return formData.value.title
        .toLowerCase()
        .replace(/[^\w\s-]/g, '') // Remove non-word chars (except spaces and hyphens)
        .replace(/\s+/g, '-') // Replace spaces with hyphens
        .replace(/--+/g, '-') // Replace multiple hyphens with single hyphen
        .trim(); // Trim hyphens from start and end
});
const formData = ref({
    id: '',
    title: '',
    slug: '',
    postImage: null,
    createDate: '',
    content: '<p>Add Post here.</p>',
});
   const submitForm = async() => {
      const submissionData = {
        ...formData.value,
        id: Date.now(),
        slug: slugComputed.value // Use the computed slug
      };
     console.log("submissionData:", submissionData)
     try{
      const response = await fetch(`${API_URL}/api/upsertExample`, {
        method: 'POST',
        headers: {
        'Content-Type': 'application/json',
        },
        body: JSON.stringify({ id: `${submissionData.id}`, name: `${submissionData}` }),
        const data = await response.json();
        console.log("data:", data);
     }catch(error){
      console.log("error:",error)
     }
    };
</script>
