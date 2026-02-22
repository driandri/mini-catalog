<script setup>
import { computed } from 'vue'

const props = defineProps({
  item: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['edit', 'delete'])

const defaultImage = 'https://images.unsplash.com/photo-1586023492125-27b2c045efd7?w=500&q=80'
const displayImage = computed(() => props.item.image || defaultImage)

const formatRupiah = (number) => {
  return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', maximumFractionDigits: 0 }).format(number)
}

const handleEdit = () => emit('edit', props.item)
const handleDelete = () => emit('delete', props.item.id)
</script>

<template>
  <div class="catalog-card glass-panel flex flex-col">
    <div class="card-image-wrapper">
      <img :src="displayImage" :alt="item.name" class="card-image" loading="lazy">
      <div class="card-overlay">
        <span v-if="item.status" class="badge badge-status" :class="item.status === 'Tersedia' ? 'badge-success' : 'badge-warning'">
          {{ item.status }}
        </span>
        <span class="category-tag">{{ item.category }}</span>
      </div>
    </div>
    
    <div class="card-content flex flex-col flex-1">
      <div class="card-header justify-between items-center">
        <h3 class="product-name" :title="item.name">{{ item.name }}</h3>
        <p class="product-price">{{ formatRupiah(item.price) }}</p>
      </div>
      
      <p class="product-description">{{ item.description }}</p>
      
      <div class="card-actions flex gap-2 mt-auto">
        <button @click="handleEdit" class="btn btn-secondary btn-icon" title="Ubah">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"></path></svg>
          Ubah
        </button>
        <button @click="handleDelete" class="btn btn-danger btn-icon" title="Hapus">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path><line x1="10" y1="11" x2="10" y2="17"></line><line x1="14" y1="11" x2="14" y2="17"></line></svg>
          Hapus
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.catalog-card {
  overflow: hidden;
  transition: transform var(--transition-normal), box-shadow var(--transition-normal);
  display: flex;
  flex-direction: column;
  height: 100%;
}

.catalog-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 12px 40px 0 rgba(0, 0, 0, 0.4), 0 0 15px rgba(99, 102, 241, 0.2);
  border-color: rgba(99, 102, 241, 0.3);
}

.card-image-wrapper {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.card-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform var(--transition-slow);
}

.catalog-card:hover .card-image {
  transform: scale(1.05);
}

.card-overlay {
  position: absolute;
  top: 0; left: 0; right: 0;
  padding: 12px;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  background: linear-gradient(to bottom, rgba(0,0,0,0.6) 0%, transparent 100%);
}

.category-tag {
  background: rgba(0,0,0,0.5);
  backdrop-filter: blur(4px);
  padding: 4px 8px;
  border-radius: var(--radius-sm);
  font-size: 0.75rem;
  color: #fff;
  border: 1px solid rgba(255,255,255,0.1);
}

.badge-warning {
  background: rgba(245, 158, 11, 0.1);
  color: #fbbf24;
  border: 1px solid rgba(245, 158, 11, 0.2);
}

.card-content {
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  flex: 1;
}

.card-header {
  display: flex;
  margin-bottom: 0.5rem;
  gap: 1rem;
}

.product-name {
  font-size: 1.25rem;
  color: var(--text-primary);
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
  margin: 0;
}

.product-price {
  font-family: 'Outfit', sans-serif;
  font-weight: 700;
  color: var(--accent-primary);
  font-size: 1.2rem;
  margin: 0;
}

.product-description {
  color: var(--text-secondary);
  font-size: 0.9rem;
  margin-bottom: 1.5rem;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  flex-grow: 1;
}

.mt-auto {
  margin-top: auto;
}

.card-actions {
  display: flex;
  width: 100%;
}

.card-actions button {
  flex: 1;
}
</style>
