<template>
  <div class="w-screen h-screen flex flex-col items-center justify-center">
    <form class="flex flex-col justify-center items-center bg-red-600">
      <input class="" type="file" @change="handleFileChange" />
      <button @click.prevent="uploadFile" :disabled="!selectedFile">
        Upload File
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref } from "vue";

const uploadUrl = "https://google-drive-nodejs.onrender.com/upload-file/test"; // Replace with your API endpoint
const selectedFile = ref(null); // To store the selected file

const handleFileChange = (event) => {
  const file = event.target.files[0]; // Get the first selected file
  if (file) {
    selectedFile.value = file; // Store the selected file
    console.log("Selected file:", selectedFile.value); // Log the selected file details
  }
};

const uploadFile = async () => {
  if (!selectedFile.value) {
    console.error("No file selected.");
    return;
  }

  const formData = new FormData(); // Create a new FormData instance
  formData.append("file", selectedFile.value); // Append the selected file

  // Log the FormData contents for debugging
  for (const [key, value] of formData.entries()) {
    console.log(`${key}:`, value);
  }

  // Use useFetch to upload the file
  const { data, error } = await useFetch(uploadUrl, {
    method: "POST",
    body: formData,
  });

  if (error.value) {
    console.error("Upload Error:", error);
  } else {
    console.log("Upload Success:", data.value);
  }
};
</script>
