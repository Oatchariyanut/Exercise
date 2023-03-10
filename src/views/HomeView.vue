<script setup>
import banner from "@/assets/banner.png";
import { ref } from "vue";

import { initializeApp } from "firebase/app";
import { getFirestore, collection, getDocs } from "firebase/firestore";

const isLoading = ref(true);
const db = getFirestore(initializeApp);
const docRef = collection(db, "Exercise");
const exercise = ref([]);
const getCollections = (docRef) => {
  return new Promise((resolve, reject) => {
    getDocs(docRef)
      .then((collections) => {
        collections.docs.forEach((val) => {
          exercise.value.push(val.data());
        });
        isLoading.value = false;
        resolve(collections.docs);
      })
      .catch((err) => {
        isLoading.value = true;
        alert(err.message);
      }),
      reject;
  });
};
getCollections(docRef);
</script>

<template>
  <main>
    <div class="container">
      <img class="web-banner" :src="banner" alt="" />

      <div class="p-1" style="background: brown">
        <RouterLink to="/add" class="btn btn-primary w-100" type="button"
          >เพิ่มรายการออกกำลังกาย</RouterLink
        >
      </div>
      <div class="my-3">
        <table class="table">
          <tbody>
            <tr>
              <td>วันที่</td>
              <td>กิจกรรม</td>
              <td>ระยะเวลา (นาที)</td>
              <td>แคลลอรี่</td>
              <td>หมายเหตุ</td>
            </tr>

            <tr v-for="item in exercise" :key="item.id">
              <td>{{ item.time }}</td>
              <td>{{ item.activity }}</td>
              <td>{{ item.duration }}</td>
              <td>{{ item.calories }}</td>
              <td>{{ item.note }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </main>
</template>

<style scoped>
.web-banner {
  width: 100%;
}
</style>
