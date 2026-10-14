<script setup lang="ts">
defineProps<{
  description: string;
  ingredients?: string[];
}>();
</script>

<template>
  <div class="product-description-card">
    <!-- Deskripsi Menu -->
    <div class="desc-block">
      <h3 class="section-title">
        <span class="icon">📖</span>
        <span>Deskripsi Menu</span>
      </h3>
      <p class="description-text">{{ description }}</p>
    </div>

    <!-- Bahan & Rempah -->
    <div v-if="ingredients && ingredients.length" class="ingredients-block">
      <h3 class="section-title">
        <span class="icon">🌿</span>
        <span>Bahan & Rempah</span>
      </h3>
      <div class="ingredients-wrap">
        <span
          v-for="item in ingredients"
          :key="item"
          class="ingredient-chip"
        >
          {{ item }}
        </span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.product-description-card {
  margin-top: 1.25rem;
  padding: 1.15rem 1.25rem;
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 14px;
  display: flex;
  flex-direction: column;
  gap: 1.15rem;
  width: 100%;
}

@media (min-width: 768px) {
  .product-description-card {
    margin-top: 1.5rem;
    padding: 1.25rem 1.5rem;
    background-color: #ffffff;
    border-radius: 16px;
    gap: 1.25rem;
    box-shadow: 0 2px 12px rgba(15, 23, 42, 0.03);
  }
}

.desc-block,
.ingredients-block {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.ingredients-block {
  padding-top: 0.85rem;
  border-top: 1px dashed #e2e8f0;
}

@media (min-width: 768px) {
  .ingredients-block {
    padding-top: 1rem;
  }
}

.section-title {
  display: flex;
  align-items: center;
  gap: 0.45rem;
  font-size: 0.95rem;
  font-weight: 700;
  color: #1e293b;
  margin: 0;
}

@media (min-width: 768px) {
  .section-title {
    font-size: 1rem;
  }
}

.icon {
  font-size: 1.1rem;
}

.description-text {
  font-size: 0.875rem;
  line-height: 1.6;
  color: #475569;
  margin: 0;
}

@media (min-width: 768px) {
  .description-text {
    font-size: 0.925rem;
    line-height: 1.65;
  }
}

.ingredients-wrap {
  display: flex;
  flex-wrap: wrap;
  gap: 0.4rem;
}

@media (min-width: 768px) {
  .ingredients-wrap {
    gap: 0.5rem;
  }
}

.ingredient-chip {
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  color: #334155;
  font-size: 0.775rem;
  font-weight: 600;
  padding: 0.3rem 0.65rem;
  border-radius: 8px;
  transition: all 0.2s ease;
}

@media (min-width: 768px) {
  .ingredient-chip {
    background-color: #f8fafc;
    font-size: 0.8rem;
    padding: 0.35rem 0.75rem;
  }
}

.ingredient-chip:hover {
  background-color: #fff1eb;
  border-color: #ff5a1f;
  color: #ff5a1f;
}
</style>
