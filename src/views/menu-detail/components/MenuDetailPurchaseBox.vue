<script setup lang="ts">
import { computed } from "vue";
import useFormatCurrency from "@/composables/useCurrency";

const props = defineProps<{
  price: number;
  stock: number;
  quantity: number;
}>();

const emit = defineEmits<{
  (e: "update:quantity", val: number): void;
  (e: "addToCart"): void;
}>();

const totalPrice = computed(() => props.price * props.quantity);

const handleDecrease = () => {
  if (props.quantity > 1) {
    emit("update:quantity", props.quantity - 1);
  }
};

const handleIncrease = () => {
  if (props.quantity < props.stock) {
    emit("update:quantity", props.quantity + 1);
  }
};
</script>

<template>
  <div class="purchase-box">
    <!-- Row Atas: Jumlah & Subtotal -->
    <div class="purchase-info-row">
      <div class="quantity-control-group">
        <span class="control-label">Jumlah:</span>
        <div class="quantity-counter">
          <button
            type="button"
            class="btn-qty"
            :disabled="quantity <= 1"
            @click="handleDecrease"
            title="Kurang"
          >
            −
          </button>
          <span class="qty-display">{{ quantity }}</span>
          <button
            type="button"
            class="btn-qty"
            :disabled="quantity >= stock"
            @click="handleIncrease"
            title="Tambah"
          >
            +
          </button>
        </div>
      </div>

      <div class="total-group">
        <span class="total-label">Subtotal:</span>
        <strong class="total-value">{{ useFormatCurrency(totalPrice) }}</strong>
      </div>
    </div>

    <!-- Tombol Tambah ke Keranjang -->
    <button
      type="button"
      class="btn-add-cart-detail"
      :disabled="stock <= 0"
      @click="emit('addToCart')"
    >
      <span class="cart-icon">🛒</span>
      <span>{{ stock > 0 ? '+ Tambah ke Keranjang' : 'Stok Habis' }}</span>
    </button>
  </div>
</template>

<style scoped>
.purchase-box {
  margin-top: auto;
  padding-top: 1.25rem;
  border-top: 1px dashed #e2e8f0;
  display: flex;
  flex-direction: column;
  gap: 1.15rem;
  width: 100%;
}

@media (min-width: 640px) {
  .purchase-box {
    padding-top: 1.5rem;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    justify-content: space-between;
    gap: 1.25rem;
  }
}

.purchase-info-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  gap: 1rem;
}

@media (min-width: 640px) {
  .purchase-info-row {
    width: auto;
    gap: 1.5rem;
  }
}

.quantity-control-group {
  display: flex;
  align-items: center;
  gap: 0.65rem;
}

.control-label {
  font-size: 0.85rem;
  font-weight: 600;
  color: #475569;
}

.quantity-counter {
  display: inline-flex;
  align-items: center;
  background-color: #ffffff;
  border: 1px solid #cbd5e1;
  border-radius: 9999px;
  padding: 0.2rem;
  box-shadow: 0 1px 4px rgba(15, 23, 42, 0.04);
}

.btn-qty {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  border: none;
  background-color: #f1f5f9;
  color: #1e293b;
  font-size: 1.1rem;
  font-weight: 700;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.btn-qty:hover:not(:disabled) {
  background-color: #ff5a1f;
  color: #ffffff;
}

.btn-qty:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}

.qty-display {
  min-width: 32px;
  text-align: center;
  font-weight: 700;
  font-size: 0.95rem;
  color: #1e293b;
}

.total-group {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
}

@media (min-width: 640px) {
  .total-group {
    align-items: flex-start;
  }
}

.total-label {
  font-size: 0.725rem;
  color: #64748b;
  font-weight: 500;
}

.total-value {
  font-size: 1.25rem;
  font-weight: 800;
  color: #ff5a1f;
}

.btn-add-cart-detail {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  background: linear-gradient(135deg, #ff6b35 0%, #ff3d00 100%);
  color: #ffffff;
  border: none;
  border-radius: 9999px;
  padding: 0.9rem 1.5rem;
  font-family: inherit;
  font-size: 0.95rem;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 6px 18px rgba(255, 90, 31, 0.3);
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  width: 100%;
}

@media (min-width: 640px) {
  .btn-add-cart-detail {
    width: auto;
    padding: 0.95rem 2rem;
    font-size: 1rem;
    flex-grow: 0;
  }
}

.btn-add-cart-detail:hover:not(:disabled) {
  background: #e04810;
  transform: translateY(-2px);
  box-shadow: 0 10px 24px rgba(255, 90, 31, 0.35);
}

.btn-add-cart-detail:active:not(:disabled) {
  transform: scale(0.98);
}

.btn-add-cart-detail:disabled {
  background: #cbd5e1;
  box-shadow: none;
  cursor: not-allowed;
}

.cart-icon {
  font-size: 1.1rem;
}
</style>
