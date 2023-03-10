<template>
  <div class="container mt-5">
    <div class="card border-sucess">
      <div class="card-header bg-success text-light">เพิ่มรายการออกกำลังกาย</div>
      <div class="card-body">
        <div class="row g-3">
          <div class="col-md-6">
            <div class="form-group">
              <label for="activity">กิจกรรม</label>
              <input v-model="activity" class="form-control" placeholder="กิจกรรม" type="text" />
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group">
              <label for="isIncome">แคลลอรี่ที่เผาผลาญ</label>
              <input v-model="calories" class="form-control" placeholder="แคลลอรี่" type="number" />
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group">
              <label for="title">ระยะเวลา (นาที)</label>
              <input v-model="duration" class="form-control" placeholder="ระยะเวลา" type="number" />
            </div>
          </div>
          <div class="col-md-6">
            <div class="form-group">
              <label for="title">เวลา</label>
              <input v-model="time" class="form-control" placeholder="เวลา" type="datetime-local" />
            </div>
          </div>
          <div class="col-md-12">
            <div class="form-group">
              <label for="isIncome">หมายเหตุ</label>
              <textarea v-model="note" cols="30" placeholder="เขียนหมายเหตุที่นี่" rows="10" class="form-control"></textarea>
            </div>

            <button
              class="btn mt-3 btn-primary w-100"
              type="button"
              @click="saveData"
              :disabled="isBtnLoad"
            >
              {{ btnMessage }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { addDoc, collection, getFirestore } from "@firebase/firestore";
import { initializeApp } from "@firebase/app";
import { useRouter } from "vue-router";

const activity = ref("");
const duration = ref(0);
const calories = ref(0);
const time = ref("");
const note = ref("");

const db = getFirestore(initializeApp);
const router = useRouter();

const [isBtnLoad, btnMessage] = [ref(false), ref("บันทึกข้อมูล")];

async function saveData() {
  [isBtnLoad.value, btnMessage.value] = [true, "กำลังโหลด...."];
  const docRef = {
    activity: activity.value,
    duration: duration.value,
    calories: calories.value,
    time: time.value,
    note: note.value
  };
  return addDoc(collection(db, "Exercise"), docRef).then(() => {
    router.push("/");
  }).catch(() => {
      [isBtnLoad.value, btnMessage.value] = [false, "บันทึกข้อมูล"];
      alert("Failed to save");
    });
}
</script>
