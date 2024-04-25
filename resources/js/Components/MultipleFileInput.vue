<template>
    <div>
      <input type="file" ref="fileInput" multiple @change="handleFileChange"
             class="hidden">
      <label for="fileInput"
           class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded cursor-pointer"
           @click="openFileInput">
        Select Files
     </label>
      <button @click="uploadFiles"
              class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded ml-4"
              :disabled="selectedFiles.length === 0 || loading">
        Upload
      </button>
      <ul class="mt-4">
        <li v-for="(file, index) in selectedFiles" :key="index" class="flex justify-between items-center py-2">
            <span>
                {{ file.name }}
                <FontAwesomeIcon @click="removeFile(index)" class="mx-3 text-red-500" icon="circle-xmark" />
            </span>
            <span v-if="file.uploaded" class="text-green-500">Uploaded</span>
            <span v-if="file.uploading" class="text-blue-500">{{ file.progress }}%</span>
        </li>
      </ul>
    <!-- <div v-if="loading" class="mt-4">
        <span v-if="totalProgress !== 100">Uploading... {{ totalProgress }}%</span>
        <span v-else>Upload Complete!</span>
    </div> -->
    </div>
  </template>

  <script setup>
  import { ref } from 'vue';

  const fileInput = ref(null);
  const selectedFiles = ref([]);
  const loading = ref(false);
  const totalProgress = ref(0);

  const handleFileChange = (event) => {
  const newFiles = Array.from(event.target.files).map(file => ({
    name: file.name,
    uploaded: false,
    uploading: false,
    progress: 0
  }));
  selectedFiles.value = [...selectedFiles.value, ...newFiles];
};

  const openFileInput = () => {
    fileInput.value.click();
  };


  const uploadFiles = async () => {
  loading.value = true;
  totalProgress.value = 0;

  for (let i = 0; i < selectedFiles.value.length; i++) {
    const file = selectedFiles.value[i];
    file.uploading = true;

    await new Promise(resolve => {
        const interval = setInterval(() => {
            if (file.progress >= 100) {
                clearInterval(interval);
                resolve();
            } else {
                file.progress += 20;
                totalProgress.value += 1;
            }
            }, 500);
        });

        file.uploading = false;
        file.uploaded = true;
  }

  loading.value = false;
};

const removeFile = (index) => {
  selectedFiles.value.splice(index, 1);
};
</script>
