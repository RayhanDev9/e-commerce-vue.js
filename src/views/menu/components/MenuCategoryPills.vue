<script setup lang="ts">
import type { CategoryItem } from "../data/menuData";

defineProps<{
  categories: CategoryItem[];
  modelValue: string;
}>();

const emit = defineEmits<{
  (e: "update:modelValue", value: string): void;
}>();
</script>

<template>
  <section class="category-section">
    <div class="category-pills">
      <button
        v-for="cat in categories"
        :key="cat.id"
        :class="['pill-btn', { active: modelValue === cat.id }]"
        @click="emit('update:modelValue', cat.id)"
      >
        <span class="pill-icon">{{ cat.icon }}</span>
        <span class="pill-label">{{ cat.name }}</span>
      </button>
    </div>
  </section>
</template>

<style scoped>
.category-section {
  margin-bottom: 1.5rem;
  width: 100%;
  max-width: 100%;
  min-width: 0;
}

.category-pills {
  display: flex;
  gap: 0.65rem;
  overflow-x: auto;
  padding-bottom: 0.5rem;
  scrollbar-width: none;
  -ms-overflow-style: none;
  -webkit-overflow-scrolling: touch;
  width: 100%;
  max-width: 100%;
  min-width: 0;
}

.category-pills::-webkit-scrollbar {
  display: none;
}

.pill-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.65rem 1.15rem;
  border-radius: 9999px;
  background-color: #ffffff;
  border: 1px solid #e2e8f0;
  color: #475569;
  font-family: inherit;
  font-weight: 600;
  font-size: 0.875rem;
  white-space: nowrap;
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.04);
  flex-shrink: 0;
}

@media (max-width: 480px) {
  .pill-btn {
    padding: 0.5rem 0.9rem;
    font-size: 0.825rem;
    gap: 0.4rem;
  }
}

.pill-icon {
  font-size: 1.05rem;
  line-height: 1;
  flex-shrink: 0;
}

.pill-label {
  white-space: nowrap;
  flex-shrink: 0;
}

.pill-btn:hover {
  background-color: #f8fafc;
  border-color: #cbd5e1;
  transform: translateY(-1px);
}

.pill-btn.active {
  background: linear-gradient(135deg, #ff6b35 0%, #ff3d00 100%);
  color: #ffffff;
  border-color: transparent;
  box-shadow: 0 4px 14px rgba(255, 90, 31, 0.35);
}
</style>
