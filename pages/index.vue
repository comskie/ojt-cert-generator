<script lang="ts" setup>
import jsPDF from "jspdf";

const name = ref<string>();
const loading = ref<boolean>(false);

function submit() {
  loading.value = true;
  const doc = new jsPDF({
    orientation: "landscape",
    unit: "cm",
    compress: true,
  });

  doc.addFont(
    "/fonts/GreatVibes/GreatVibes-Regular.ttf",
    "GreatVibes",
    "normal"
  );

  doc.addImage("/programming-workshop-template.png", "PNG", 0, 0, 29.7, 21);

  doc.setFont("GreatVibes").setFontSize(54.9).text(name.value, 1.4, 11.05);

  doc.save(
    `${transformToSnakeCase(name.value)}_programming_workshop_${Date.now()}.pdf`
  );
  loading.value = false;
}

function transformToSnakeCase(str: string) {
  return str
    .toLowerCase()
    .trim()
    .replace(/[^\w\s-]/g, "")
    .replace(/[\s_-]+/g, "_")
    .replace(/^-+|-+$/g, "");
}
</script>

<template>
  <main>
    <div class="h-screen flex justify-center items-center">
      <div class="max-w-md w-full">
        <form @submit.prevent="submit">
          <fieldset :disabled="loading">
            <label for="name" class="text-2xl font-semibold text-indigo-500">
              Name
            </label>
            <input
              id="name"
              placeholder="Enter your name"
              type="text"
              v-model="name"
              required
              class="mt-1 w-full text-xl rounded-md border-gray-300 focus:border-indigo-400 focus:ring focus:ring-indigo-200/50"
            />
            <div class="mt-4 flex justify-end">
              <button
                type="submit"
                class="px-4 py-2 bg-indigo-500 hover:bg-indigo-400 rounded-md text-white font-semibold focus:ring-indigo-200 focus:ring disabled:bg-indigo-200 inline-flex flex-row gap-2 justify-center items-center"
              >
                Download PDF
                <svg
                  v-if="loading"
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 20 20"
                  fill="currentColor"
                  class="w-5 h-5 animate-spin"
                >
                  <path
                    fill-rule="evenodd"
                    d="M15.312 11.424a5.5 5.5 0 01-9.201 2.466l-.312-.311h2.433a.75.75 0 000-1.5H3.989a.75.75 0 00-.75.75v4.242a.75.75 0 001.5 0v-2.43l.31.31a7 7 0 0011.712-3.138.75.75 0 00-1.449-.39zm1.23-3.723a.75.75 0 00.219-.53V2.929a.75.75 0 00-1.5 0V5.36l-.31-.31A7 7 0 003.239 8.188a.75.75 0 101.448.389A5.5 5.5 0 0113.89 6.11l.311.31h-2.432a.75.75 0 000 1.5h4.243a.75.75 0 00.53-.219z"
                    clip-rule="evenodd"
                  />
                </svg>
              </button>
            </div>
          </fieldset>
        </form>
      </div>
    </div>
  </main>
</template>
