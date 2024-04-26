<template>
    <div>
      <input type="file" ref="fileInput" @change="handleFileChange"
             class="hidden">
      <label for="fileInput"
           class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded cursor-pointer"
           @click="openFileInput">
        Select Files
     </label>
      <button @click="uploadFiles"
              class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded ml-4"
              :disabled="selectedFile === null || loading">
        Upload
      </button>
      <div class="mt-4" v-if="selectedFile != null">
          <div class="justify-start items-center py-2 mx-2">
            <div>
              <img :src="selectedFile.imageUrl" alt="" class="w-[50vh] my-3">
              <span class="mt-3">
                {{ selectedFile.name }}
                <FontAwesomeIcon @click="removeFile(index)" v-if="!loading" class="mx-3 text-red-500" icon="circle-xmark" />
              </span>
              <div>
                <span v-if="selectedFile.uploaded" class="text-green-500">Uploaded</span>
                <span v-if="selectedFile.uploading" class="text-blue-500">{{ selectedFile.progress }}%</span>
              </div>
            </div>
          </div>
        </div>
        <div v-if="isDone" class="mt-4">
            <span class="text-green-500">Upload Complete!</span>
        </div>
    </div>
  </template>

  <script setup>
  import { ref } from 'vue';

  const fileInput = ref(null);
  const selectedFile = ref(null);
  const loading = ref(false);
  const totalProgress = ref(0);
  const isDone = ref(false);

  const handleFileChange = (event) => {
      const file = event.target.files[0];
      if (file) {
        const imageUrl = URL.createObjectURL(file);
        selectedFile.value = {
          name: file.name,
          imageUrl: imageUrl,
          uploaded: false,
          uploading: false,
          progress: 0,
        };
      }
    };

  const openFileInput = () => {
    fileInput.value.click();
  };


  const uploadFiles = async () => {
   loading.value = true;
   totalProgress.value = 0;

   selectedFile.value.uploading = true;

    await new Promise(resolve => {
    const interval = setInterval(() => {
        if (selectedFile.value.progress >= 100) {
            clearInterval(interval);
            resolve();
        } else {
            selectedFile.value.progress += 20;
            totalProgress.value += 1;
        }
        }, 500);
    });

    selectedFile.value.uploading = false;
    selectedFile.value.uploaded = true;

    loading.value = false;
    selectedFile.value = null;
    isDone.value = true;
    setTimeout(() => {
        isDone.value = false;
    }, 5000);
};

const removeFile = (index) => {
    selectedFile.value = null;
};
</script>
