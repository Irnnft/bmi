<script setup>
import { ref } from 'vue'

const nama = ref('')
const jenisKelamin = ref('pria')
const tinggi = ref('')
const berat = ref('')
const hasil = ref('')
const showTips = ref(false)
const showWorkout = ref(false)
const selectedGoal = ref('')
const bmiValue = ref(0)
const kategori = ref('')

const hitungBMI = () => {
  const tb = parseFloat(tinggi.value) / 100
  const bb = parseFloat(berat.value)
  bmiValue.value = bb / (tb * tb)

  if (bmiValue.value < 18.5) {
    kategori.value = 'Kurus'
    selectedGoal.value = 'bulking' // Auto select bulking for underweight
  } else if (bmiValue.value < 25) {
    kategori.value = 'Normal'
  } else if (bmiValue.value < 30) {
    kategori.value = 'Gemuk'
    selectedGoal.value = 'cutting' // Auto select cutting for overweight
  } else {
    kategori.value = 'Obesitas'
    selectedGoal.value = 'extreme' // Auto select extreme for obese
  }

  hasil.value = `${nama.value} (${jenisKelamin.value === 'pria' ? 'Laki-laki' : 'Perempuan'}), BMI Anda: ${bmiValue.value.toFixed(2)} (${kategori.value})`
  showTips.value = true
  showWorkout.value = false
}

const tips = {
  Kurus: [
    "Targetkan surplus kalori 300-500 kalori/hari",
    "Fokus pada makanan padat nutrisi: alpukat, kacang-kacangan, daging merah",
    "Latihan beban 3-4x/minggu untuk membangun otot",
    "Konsumsi protein 1.6-2.2g per kg berat badan"
  ],
  Normal: [
    "Pertahankan pola makan seimbang",
    "Monitor berat badan mingguan",
    "Pilih tujuan fitness sesuai keinginan"
  ],
  Gemuk: [
    "Defisit kalori 300-500 kalori/hari",
    "Hindari gula tambahan dan makanan olahan",
    "Latihan kardio 3-4x/minggu",
    "Perbanyak protein untuk menjaga massa otot"
  ],
  Obesitas: [
    "Konsultasi dokter sebelum memulai program",
    "Defisit kalori 500-750 kalori/hari",
    "Fokus pada perubahan gaya hidup permanen",
    "Mulai dengan olahraga low-impact (jalan, berenang)"
  ]
}

const workoutPlans = {
  bulking: [
    "Senin: Dada & Trisep (Bench press 4x8, Incline dumbbell 3x10, Dips 3xmax)",
    "Selasa: Punggung & Bisep (Pull-ups 4x8, Barbell row 3x10, Deadlift 3x5)",
    "Rabu: Istirahat",
    "Kamis: Kaki (Squat 4x8, Leg press 3x10, Calf raises 4x15)",
    "Jumat: Bahu (Overhead press 4x8, Lateral raises 3x12, Shrugs 3x15)",
    "Sabtu: Full body (Compound movements)",
    "Minggu: Istirahat"
  ],
  cutting: [
    "Senin: HIIT 20min + Latihan Dada (Circuit training)",
    "Selasa: LISS Cardio 45min + Core workout",
    "Rabu: Latihan Beban Full Body (Supersets)",
    "Kamis: HIIT 20min + Latihan Punggung",
    "Jumat: Yoga/Pilates untuk fleksibilitas",
    "Sabtu: Latihan Beban Intensitas Tinggi",
    "Minggu: Istirahat Aktif (jalan santai)"
  ],
  extreme: [
    "Senin: Cardio pagi 30min + Full body resistance training",
    "Selasa: Cardio LISS 60min (jalan cepat/incline)",
    "Rabu: Latihan Beban Full Body (Circuit training)",
    "Kamis: Cardio HIIT 30min + Core workout",
    "Jumat: Yoga untuk mobilitas",
    "Sabtu: Aktivitas outdoor intensitas sedang",
    "Minggu: Istirahat total"
  ]
}
</script>

<template>
  <div class="container d-flex justify-content-center align-items-center min-vh-100 py-4">
    <div class="row w-100" style="background-color: #3a5a40; padding: 30px; border-radius: 15px; color: white; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; box-shadow: 0 10px 30px rgba(0,0,0,0.2); max-width: 1200px;">
      <h2 class="text-center mb-4" style="font-weight: 700; color: #d8f3dc;">Body Mass Index Calculator</h2>
      <div class="col-lg-6">
        <div class="mb-3">
          <label class="form-label fw-bold">Nama</label>
          <input v-model="nama" type="text" class="form-control rounded-pill shadow" style="background-color: #f8f9fa;">
        </div>
        <div class="mb-3">
          <label class="form-label fw-bold">Jenis Kelamin</label>
          <div class="d-flex gap-3">
            <div class="form-check">
              <input v-model="jenisKelamin" class="form-check-input" type="radio" id="pria" value="pria" checked>
              <label class="form-check-label" for="pria">
                Laki-laki
              </label>
            </div>
            <div class="form-check">
              <input v-model="jenisKelamin" class="form-check-input" type="radio" id="wanita" value="wanita">
              <label class="form-check-label" for="wanita">
                Perempuan
              </label>
            </div>
          </div>
        </div>
        <div class="mb-3">
          <label class="form-label fw-bold">Tinggi Badan (cm)</label>
          <input v-model="tinggi" type="number" class="form-control rounded-pill shadow" style="background-color: #f8f9fa;">
        </div>
        <div class="mb-3">
          <label class="form-label fw-bold">Berat Badan (kg)</label>
          <input v-model="berat" type="number" class="form-control rounded-pill shadow" style="background-color: #f8f9fa;">
        </div>
        <button class="btn btn-success rounded-pill shadow mt-2 px-4 py-2 fw-bold" @click="hitungBMI" style="background-color: #2d6a4f; border: none;">Hitung BMI</button>
      </div>

      <div class="col-lg-6 mt-4 mt-lg-0">
        <div class="p-4 rounded-4 shadow h-100" style="background-color: #d8f3dc; color: #1b263b;">
          <h5 class="text-center mb-3 fw-bold">Hasil</h5>
          <p style="white-space: pre-line; min-height: 60px;">{{ hasil }}</p>
          
          <div v-if="showTips" class="mt-3">
            <h6 class="fw-bold">Tips untuk Anda:</h6>
            <ul>
              <li v-for="(tip, index) in tips[kategori]" :key="index">{{ tip }}</li>
            </ul>
            
            <div v-if="kategori === 'Normal'" class="mt-3">
              <h6 class="fw-bold">Pilih Tujuan Anda:</h6>
              <div class="d-flex gap-3 flex-wrap">
                <button @click="selectedGoal = 'bulking'" class="btn btn-sm rounded-pill" :class="{'btn-success': selectedGoal === 'bulking', 'btn-outline-success': selectedGoal !== 'bulking'}">Bulking (Naik Massa Otot)</button>
                <button @click="selectedGoal = 'cutting'" class="btn btn-sm rounded-pill" :class="{'btn-success': selectedGoal === 'cutting', 'btn-outline-success': selectedGoal !== 'cutting'}">Cutting (Turun Lemak)</button>
              </div>
            </div>
            
            <button v-if="kategori !== 'Obesitas' || selectedGoal" @click="showWorkout = !showWorkout" class="btn btn-success rounded-pill mt-3 px-4 py-2 fw-bold" style="background-color: #2d6a4f; border: none;">
              {{ showWorkout ? 'Sembunyikan' : 'Tampilkan' }} Jadwal Olahraga
            </button>
            
            <div v-if="showWorkout" class="mt-3">
              <h6 class="fw-bold">Jadwal Olahraga:</h6>
              <ul class="list-group">
                <li v-for="(workout, index) in workoutPlans[selectedGoal || (kategori === 'Obesitas' ? 'extreme' : kategori === 'Gemuk' ? 'cutting' : 'bulking')]" :key="index" class="list-group-item rounded mb-2">
                  {{ workout }}
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.btn-success:hover {
  background-color: #1b4332 !important;
  transform: translateY(-2px);
  transition: all 0.3s ease;
}

.list-group-item {
  background-color: #f8f9fa;
  border: 1px solid #dee2e6;
  font-size: 0.9rem;
}

@media (max-width: 768px) {
  .container {
    padding: 15px;
  }
  .row {
    padding: 15px !important;
  }
  .btn-sm {
    width: 100%;
    margin-bottom: 5px;
  }
}
</style>