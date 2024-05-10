<template>
  <!-- Menggunakan div sebagai pembungkus utama -->
  <div class="background">
    <h1>Nurul Hayana Putri</h1>
    <h2>NPM 223510821</h2>

    <!-- Menampilkan kegiatan yang sudah ada dalam sebuah tabel -->
    <table>
      <thead>
        <tr>
          <th>Kegiatan</th>
          <th>Status</th>
          <th>Tindakan</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="activity in activities" :key="activity.id" :class="{ completed: activity.completed }">
          <td>
            <!-- Display edit input if this activity is being edited -->
            <template v-if="editingActivity.id === activity.id">
              <input type="text" v-model="editingActivity.name">
            </template>
            <!-- Otherwise, show activity name -->
            <template v-else>
              <span :style="{ 'text-decoration': activity.completed ? 'line-through' : 'none' }">{{ activity.name }}</span>
            </template>
          </td>
          <td>
            <span v-if="activity.completed">Telah Selesai</span>
            <span v-else>Belum Selesai</span>
            <input type="checkbox" v-model="activity.completed">
          </td>
          <td>
            <!-- Show edit buttons if this activity is being edited -->
            <template v-if="editingActivity.id === activity.id">
              <button @click="saveActivity">Simpan</button>
              <button @click="cancelEdit">Batal</button>
            </template>
            <!-- Show regular buttons if not in edit mode -->
            <template v-else>
              <button @click="editActivity(activity)">Edit</button>
              <button @click="cancelActivity(activity.id)">Hapus</button>
            </template>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Form untuk menambahkan kegiatan baru -->
    <form @submit.prevent="addActivity">
      <input type="text" v-model="newActivity" placeholder="Masukkan kegiatan baru">
      <button type="submit">Tambahkan</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// Data contoh untuk kegiatan
const activities = ref([
  { id: 1, name: 'Kegiatan 1', completed: false },
  { id: 2, name: 'Kegiatan 2', completed: false }
]);

// State untuk mengelola kegiatan yang sedang diedit
const editingActivity = ref({ id: null, name: '' });

// Fungsi untuk memulai mode edit kegiatan
function editActivity(activity) {
  editingActivity.value = { ...activity };
}

// Fungsi untuk menyimpan perubahan kegiatan yang sedang diedit
function saveActivity() {
  const index = activities.value.findIndex(activity => activity.id === editingActivity.value.id);
  if (index !== -1) {
    activities.value[index].name = editingActivity.value.name;
    // Keluar dari mode edit setelah menyimpan
    cancelEdit();
  }
}

// Fungsi untuk membatalkan mode edit
function cancelEdit() {
  editingActivity.value = { id: null, name: '' };
}

// Fungsi untuk menambahkan kegiatan baru
function addActivity() {
  if (newActivity.value.trim() !== '') {
    activities.value.push({ id: Date.now(), name: newActivity.value, completed: false });
    newActivity.value = '';
  }
}

// Fungsi untuk menghapus kegiatan
function cancelActivity(id) {
  const index = activities.value.findIndex(activity => activity.id === id);
  if (index !== -1) {
    activities.value.splice(index, 1);
  }
}
</script>

<style scoped>
/* CSS untuk pembungkus utama */

.h1 {
  text-align: center;
}
.background {
  background-image: url(3.jpg);
  background-repeat: no-repeat;
  background-size: cover; /* Fill the entire background area */
  min-height: 100vh;
  padding: 20px;
}

/* Styling for completed activities */
.completed {
  background-color: red;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid black;
  padding: 8px;
  text-align: left;
}

th {
  background-color: white;
}

/* Adding strikethrough for completed activities */
.completed span {
  text-decoration: line-through;
}

/* CSS for footer */
footer {
  background-color: #333;
  color: white;
  text-align: center;
  padding: 20px 0;
  position: absolute;
  bottom: 0;
  width: 100%;
}

.Link {
  text-align: center;
}
</style>
