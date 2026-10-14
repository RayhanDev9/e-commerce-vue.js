<script setup lang="ts">
import { useRouter, RouterLink } from "vue-router";

defineProps<{
  productName?: string;
}>();

const router = useRouter();
</script>

<template>
  <nav class="nav-top" aria-label="Breadcrumb">
    <button class="btn-back" @click="router.back()" title="Kembali">
      <span class="back-arrow">←</span>
      <span>Kembali</span>
    </button>

    <div class="breadcrumb">
      <RouterLink to="/menu" class="breadcrumb-link">Menu</RouterLink>
      <span class="breadcrumb-separator">/</span>
      <span class="breadcrumb-current">{{ productName || "Detail Produk" }}</span>
    </div>
  </nav>
</template>

<style scoped>
.nav-top {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1.25rem;
  width: 100%;
}

.btn-back {
  display: inline-flex;
  align-items: center;
  gap: 0.45rem;
  background-color: #ffffff;
  color: #1e293b;
  border: 1px solid #e2e8f0;
  border-radius: 9999px;
  padding: 0.5rem 1rem;
  font-family: inherit;
  font-size: 0.85rem;
  font-weight: 600;
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(15, 23, 42, 0.04);
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  flex-shrink: 0;
}

.btn-back:hover {
  background-color: #fff1eb;
  color: #ff5a1f;
  border-color: #ff5a1f;
  transform: translateX(-2px);
}

.back-arrow {
  font-size: 1.05rem;
  line-height: 1;
}

.breadcrumb {
  display: flex;
  align-items: center;
  gap: 0.45rem;
  font-size: 0.85rem;
  color: #64748b;
  min-width: 0;
  overflow: hidden;
}

.breadcrumb-link {
  color: #64748b;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s ease;
  flex-shrink: 0;
}

.breadcrumb-link:hover {
  color: #ff5a1f;
}

.breadcrumb-separator {
  color: #94a3b8;
  flex-shrink: 0;
}

.breadcrumb-current {
  color: #1e293b;
  font-weight: 600;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  min-width: 0;
}

@media (max-width: 640px) {
  .nav-top {
    gap: 0.75rem;
    margin-bottom: 1rem;
  }
  .btn-back {
    padding: 0.45rem 0.85rem;
    font-size: 0.8rem;
  }
  .breadcrumb {
    font-size: 0.8rem;
  }
}
</style>
