<script lang="ts" setup>
import { now } from "@vueuse/shared";
import jsPDF from "jspdf";

const router = useRouter();

const name = ref<string>();

function downloadPdf() {
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
    `${transformToSnakeCase(name.value)}_programming_workshop_${Math.floor(
      Date.now() / 1000
    )}.pdf`
  );
}

const transformToSnakeCase = (str: string) =>
  str
    .toLowerCase()
    .trim()
    .replace(/[^\w\s-]/g, "")
    .replace(/[\s_-]+/g, "_")
    .replace(/^-+|-+$/g, "");
</script>

<template>
  <main>
    <div class="h-screen flex justify-center items-center">
      <div class="max-w-md w-full">
        <form @submit.prevent="downloadPdf" method="get">
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
              class="px-4 py-2 bg-indigo-500 hover:bg-indigo-400 rounded-md text-white font-semibold focus:ring-indigo-200 focus:ring"
            >
              Generate
            </button>
          </div>
        </form>
      </div>
    </div>
  </main>
</template>
