<script setup lang="ts">
import { reactive, computed, onUpdated } from "vue";
import { supabase } from "../supabase";
import Swal from "sweetalert2";

const registration = reactive({
  fullname: "",
  studentId: "",
  phone: "",
  visionMission: "",
});

const clearForm = () => {
  registration.fullname = "";
  registration.studentId = "";
  registration.phone = "";
  registration.visionMission = "";
};

const isValid = computed(() => {
  return (
    registration.fullname.length > 1 &&
    registration.studentId.includes("41037006") &&
    registration.phone.length > 1 &&
    registration.visionMission !== ""
  );
});

const submitData = async () => {
  try {
    const { error } = await supabase.from("registration").insert([
      {
        fullname: registration.fullname,
        studentId: registration.studentId,
        phone: registration.phone,
        visionMission: registration.visionMission,
      },
    ]);
    clearForm();
    if (error) throw error;
    Swal.fire("Success", "Terimakasih atas Partisipasi nya", "success");
  } catch (error: any) {
    console.log(error);
    Swal.fire("Error :(", `${error.message}`, "error");
  }
};
</script>
<template>
  <div class="w-full max-w-xs">
    <form
      @submit.prevent="submitData()"
      class="bg-white shadow-xl border-3 border-gray-200 rounded px-8 pt-6 pb-8 mb-4"
    >
      <div class="mb-4">
        <label class="block text-black text-sm font-bold mb-2" for="fullname">
          Nama Lengkap
        </label>
        <input
          v-model="registration.fullname"
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          type="text"
          placeholder="Masukan Nama Lengkap "
        />
      </div>
      <div class="mb-6">
        <label class="block text-black text-sm font-bold mb-2" for="nim">
          Nomor Induk Mahasiswa
        </label>
        <input
          v-model="registration.studentId"
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          type="text"
          placeholder="Masukan NIM "
        />
      </div>
      <div class="mb-6">
        <label class="block text-black text-sm font-bold mb-2" for="whatsapp">
          Nomor Whatsapp
        </label>
        <input
          v-model="registration.phone"
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
          type="text"
          placeholder="Masukan Nomor Whatsapp "
        />
      </div>
      <div class="mb-6">
        <label class="block text-black text-sm font-bold mb-2" for="Visi Misi">
          Visi Misi
        </label>
        <textarea
          v-model="registration.visionMission"
          rows="4"
          class="block p-2.5 w-full text-gray-700 rounded-lg border border-gray-300 leading-tight focus:outline-none focus:shadow-outline"
          placeholder="Masukan Visi dan Misi"
        ></textarea>
      </div>
      <div class="flex items-center justify-between">
        <button
          :disabled="!isValid"
          class="bg-blue-500 disabled:bg-gray-300 disabled:text-gray-600 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="submit"
        >
          Submit
        </button>
      </div>
    </form>
    <p class="text-center text-gray-200 text-xs">
      &copy;2022 by Fahmi. All rights reserved.
    </p>
  </div>
</template>
