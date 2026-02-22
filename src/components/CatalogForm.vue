<script setup>
import { ref, watch, onMounted } from 'vue'

const props = defineProps({
  initialData: {
    type: Object,
    default: null
  }
})

const emit = defineEmits(['save', 'cancel'])

const formData = ref({
  name: '',
  price: '',
  category: '',
  description: '',
  status: 'Tersedia',
  image: ''
})

const isSubmitting = ref(false)

// Init form if editing
onMounted(() => {
  if (props.initialData) {
    formData.value = { ...props.initialData }
  }
})

watch(() => props.initialData, (newVal) => {
  if (newVal) {
    formData.value = { ...newVal }
  } else {
    // Reset form
    formData.value = {
      name: '',
      price: '',
      category: '',
      description: '',
      status: 'Tersedia',
      image: ''
    }
  }
}, { immediate: true })

const handleSubmit = () => {
  isSubmitting.value = true
  
  // Basic validation (can be enhanced)
  if (!formData.value.name || !formData.value.price) {
    alert('Nama dan Harga wajib diisi!')
    isSubmitting.value = false
    return
  }

  // Ensure price is numeric
  const dataToSave = {
    ...formData.value,
    price: parseFloat(formData.value.price) || 0
  }

  // Simulate network delay for premium feel
  setTimeout(() => {
    emit('save', dataToSave)
    isSubmitting.value = false
  }, 400)
}
</script>

<template>
  <form @submit.prevent="handleSubmit" class="catalog-form">
    <div class="form-grid">
      <div class="input-group full-width">
        <label class="input-label" for="name">Nama Produk *</label>
        <input 
          id="name" 
          v-model="formData.name" 
          type="text" 
          class="form-input" 
          placeholder="cth. Headphone Nirkabel Premium" 
          required
        >
      </div>
      
      <div class="input-group">
        <label class="input-label" for="price">Harga (Rp) *</label>
        <input 
          id="price" 
          v-model="formData.price" 
          type="number" 
          min="0" 
          step="1000" 
          class="form-input" 
          placeholder="0" 
          required
        >
      </div>
      
      <div class="input-group">
        <label class="input-label" for="category">Kategori</label>
        <input 
          id="category" 
          v-model="formData.category" 
          type="text" 
          class="form-input" 
          placeholder="cth. Elektronik"
        >
      </div>
      
      <div class="input-group">
        <label class="input-label" for="status">Status Stok</label>
        <select id="status" v-model="formData.status" class="form-input select-input">
          <option value="Tersedia">Tersedia</option>
          <option value="Terbatas">Terbatas</option>
          <option value="Habis">Habis</option>
        </select>
      </div>

      <div class="input-group">
        <label class="input-label" for="image">URL Gambar</label>
        <input 
          id="image" 
          v-model="formData.image" 
          type="url" 
          class="form-input" 
          placeholder="https://..."
        >
      </div>
      
      <div class="input-group full-width">
        <label class="input-label" for="description">Deskripsi</label>
        <textarea 
          id="description" 
          v-model="formData.description" 
          class="form-input" 
          placeholder="Deskripsi detail produk..."
          rows="3"
        ></textarea>
      </div>
    </div>
    
    <div class="form-actions flex justify-between gap-4 mt-6">
      <button type="button" @click="$emit('cancel')" class="btn btn-secondary flex-1">Batal</button>
      <button type="submit" class="btn btn-primary flex-1" :disabled="isSubmitting">
        {{ isSubmitting ? 'Menyimpan...' : (initialData ? 'Perbarui Produk' : 'Tambah Produk') }}
      </button>
    </div>
  </form>
</template>

<style scoped>
.catalog-form {
  display: flex;
  flex-direction: column;
}

.form-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.full-width {
  grid-column: 1 / -1;
}

.select-input {
  appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' fill='%2394a3b8' viewBox='0 0 16 16'%3E%3Cpath d='M7.247 11.14 2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: calc(100% - 16px) center;
  padding-right: 40px;
}

.select-input option {
  background: var(--bg-primary);
  color: var(--text-primary);
}

.mt-6 {
  margin-top: 1.5rem;
}

.flex-1 {
  flex: 1;
}

.btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

@media (max-width: 600px) {
  .form-grid {
    grid-template-columns: 1fr;
  }
}
</style>
