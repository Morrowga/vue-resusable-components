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
      <div class="mt-4 grid grid-cols-3 gap-10">
          <div v-for="(file, index) in selectedFiles" :key="index" class="flex flex-col justify-between items-center py-2 mx-2">
            <div>
              <img :src="file.imageUrl" alt="" class="w-[50vh] my-3">
              <span class="mt-3">
                {{ file.name }}
                <FontAwesomeIcon @click="removeFile(index)" v-if="!loading" class="mx-3 text-red-500" icon="circle-xmark" />
              </span>
              <div>
                <span v-if="file.uploaded" class="text-green-500">Uploaded</span>
                <span v-if="file.uploading" class="text-blue-500">{{ file.progress }}%</span>
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
  const selectedFiles = ref([]);
  const loading = ref(false);
  const isDone  = ref(false);
  const totalProgress = ref(0);

  const handleFileChange = (event) => {
    const newFiles = Array.from(event.target.files).map(file => {
      const imageUrl = URL.createObjectURL(file);
      return {
        name: file.name,
        uploaded: false,
        uploading: false,
        progress: 0,
        imageUrl: imageUrl
      };
    });
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
  isDone.value = true;
  selectedFiles.value = [];
  
  setTimeout(() => {
    isDone.value = false;
  }, 5000);
};

const removeFile = (index) => {
  selectedFiles.value.splice(index, 1);
};
</script>
