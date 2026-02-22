<script setup>
import { ref, onMounted, watch } from 'vue'
import CatalogItem from './components/CatalogItem.vue'
import CatalogForm from './components/CatalogForm.vue'

// State
const items = ref([])
const isModalOpen = ref(false)
const editingItem = ref(null)
const searchQuery = ref('')

// Constants for LocalStorage
const STORAGE_KEY = 'mini-katalog-data'

// Load from LocalStorage
onMounted(() => {
  const savedData = localStorage.getItem(STORAGE_KEY)
  if (savedData) {
    try {
      items.value = JSON.parse(savedData)
    } catch (e) {
      console.error('Failed to parse data', e)
    }
  } else {
    // Initial dummy data to make it look good
    items.value = [
      { id: 1, name: 'Headphone Nirkabel Premium', price: 2990000, category: 'Elektronik', description: 'Nikmati suara imersif dengan fitur active noise cancellation.', status: 'Tersedia', image: 'https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=500&q=80' },
      { id: 2, name: 'Kursi Kantor Ergonomis', price: 1990000, category: 'Furnitur', description: 'Dukungan lumbal yang dapat disesuaikan dan material jaring bernapas untuk waktu lama.', status: 'Terbatas', image: 'https://images.unsplash.com/photo-1505843490538-5133c6c7d0e1?w=500&q=80' },
      { id: 3, name: 'Jam Tangan Kebugaran Pintar', price: 1490000, category: 'Aksesoris Pintar', description: 'Lacak kesehatan, detak jantung, dan notifikasi Anda saat bepergian.', status: 'Tersedia', image: 'https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=500&q=80' }
    ]
  }
})

// Save to LocalStorage whenever items change
watch(items, (newVal) => {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(newVal))
}, { deep: true })

// CRUD Operations
const saveItem = (itemData) => {
  if (editingItem.value) {
    // Update
    const index = items.value.findIndex(i => i.id === editingItem.value.id)
    if (index !== -1) {
      items.value[index] = { ...itemData, id: editingItem.value.id }
    }
  } else {
    // Create
    const newId = items.value.length > 0 ? Math.max(...items.value.map(i => i.id)) + 1 : 1
    items.value.push({ ...itemData, id: newId })
  }
  closeModal()
}

const deleteItem = (id) => {
  if (confirm('Apakah Anda yakin ingin menghapus produk ini?')) {
    items.value = items.value.filter(item => item.id !== id)
  }
}

const editItem = (item) => {
  editingItem.value = { ...item }
  isModalOpen.value = true
}

// Modal Handlers
const openAddModal = () => {
  editingItem.value = null
  isModalOpen.value = true
}

const closeModal = () => {
  isModalOpen.value = false
  setTimeout(() => {
    editingItem.value = null
  }, 300) // Wait for transition
}
</script>

<template>
  <div class="app-container">
    <header class="header">
      <div class="header-content flex items-center justify-between">
        <div>
          <h1 class="logo text-gradient">Gzn Catalog</h1>
          <p class="subtitle">Manajemen inventaris digital tanpa batas</p>
        </div>
        
        <div class="header-actions flex gap-4">
          <input 
            v-model="searchQuery" 
            type="text" 
            class="form-input search-input" 
            placeholder="Cari produk..."
          >
          <button @click="openAddModal" class="btn btn-primary">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
            Tambah Produk Baru
          </button>
        </div>
      </div>
    </header>

    <main class="main-content">
      <div v-if="items.length === 0" class="empty-state glass-panel">
        <h2>Tidak ada produk ditemukan di katalog.</h2>
        <p>Mulai dengan menambahkan produk pertama Anda!</p>
        <button @click="openAddModal" class="btn btn-primary mt-4">Tambah Produk</button>
      </div>
      
      <div v-else class="grid katalog-grid">
        <CatalogItem 
          v-for="item in items.filter(i => i.name.toLowerCase().includes(searchQuery.toLowerCase()))" 
          :key="item.id" 
          :item="item"
          @edit="editItem(item)"
          @delete="deleteItem(item.id)"
        />
      </div>
    </main>

    <!-- Modal for Form -->
    <div class="modal-overlay" :class="{ active: isModalOpen }" @click.self="closeModal">
      <div class="modal-content glass-panel">
        <div class="modal-header flex justify-between items-center mb-6">
          <h2 class="text-gradient">{{ editingItem ? 'Edit Produk' : 'Tambah Produk Baru' }}</h2>
          <button @click="closeModal" class="close-btn">&times;</button>
        </div>
        
        <CatalogForm 
          :initial-data="editingItem" 
          @save="saveItem" 
          @cancel="closeModal" 
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.header {
  margin-bottom: 3rem;
  padding-bottom: 2rem;
  border-bottom: 1px solid var(--surface-border);
}

.logo {
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
}

.subtitle {
  color: var(--text-secondary);
  font-weight: 300;
}

.search-input {
  width: 250px;
}

.empty-state {
  text-align: center;
  padding: 4rem 2rem;
  margin-top: 2rem;
}

.empty-state h2 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
  color: var(--text-primary);
}

.empty-state p {
  color: var(--text-secondary);
}

.mt-4 {
  margin-top: 1rem;
}

.mb-6 {
  margin-bottom: 1.5rem;
}

.close-btn {
  background: transparent;
  border: none;
  color: var(--text-secondary);
  font-size: 2rem;
  cursor: pointer;
  line-height: 1;
  transition: color var(--transition-fast);
}

.close-btn:hover {
  color: var(--danger);
}

@media (max-width: 768px) {
  .header-content {
    flex-direction: column;
    align-items: flex-start;
    gap: 1.5rem;
  }
  
  .header-actions {
    width: 100%;
    flex-direction: column;
  }
  
  .search-input {
    width: 100%;
  }
}
</style>
