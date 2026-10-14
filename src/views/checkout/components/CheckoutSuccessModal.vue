<script setup lang="ts">
import { useRouter } from "vue-router";
import useFormatCurrency from "@/composables/useCurrency";

export interface OrderSuccessDetail {
  orderId: string;
  customerName: string;
  phone: string;
  address: string;
  paymentMethod: string;
  totalPayment: number;
  deliveryEstimate: string;
  itemsCount: number;
}

defineProps<{
  order: OrderSuccessDetail;
}>();

const router = useRouter();

const handleBackToMenu = () => {
  router.push("/menu");
};
</script>

<template>
  <div class="modal-overlay" role="dialog" aria-modal="true">
    <div class="modal-card">
      <div class="success-icon-wrap">
        <span class="success-icon">🎉</span>
      </div>

      <h2 class="success-title">Pesanan Berhasil Dibuat!</h2>
      <p class="success-desc">
        Restoran kami sedang menyiapkan hidangan lezat Anda dengan penuh cinta.
      </p>

      <div class="order-receipt-box">
        <div class="receipt-row">
          <span class="label">Nomor Pesanan:</span>
          <strong class="value order-id">{{ order.orderId }}</strong>
        </div>

        <div class="receipt-row">
          <span class="label">Penerima:</span>
          <span class="value">{{ order.customerName }} ({{ order.phone }})</span>
        </div>

        <div class="receipt-row">
          <span class="label">Alamat Antar:</span>
          <span class="value address-val">{{ order.address }}</span>
        </div>

        <div class="receipt-divider"></div>

        <div class="receipt-row">
          <span class="label">Metode Pembayaran:</span>
          <span class="value">{{ order.paymentMethod.toUpperCase() }}</span>
        </div>

        <div class="receipt-row">
          <span class="label">Estimasi Tiba:</span>
          <span class="value arrival-val">⏱️ {{ order.deliveryEstimate }}</span>
        </div>

        <div class="receipt-row total-highlight">
          <span class="label">Total Pembayaran:</span>
          <strong class="value price">{{ useFormatCurrency(order.totalPayment) }}</strong>
        </div>
      </div>

      <button type="button" class="btn-primary-finish" @click="handleBackToMenu">
        🍽️ Belanja Menu Lainnya
      </button>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(15, 23, 42, 0.65);
  backdrop-filter: blur(6px);
  -webkit-backdrop-filter: blur(6px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  z-index: 999;
  animation: fadeIn 0.25s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.modal-card {
  background-color: #ffffff;
  border-radius: 16px;
  max-width: 480px;
  width: 100%;
  padding: 1.5rem 1.15rem;
  text-align: center;
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.2);
  border: 1px solid #e2e8f0;
  max-height: 92vh;
  overflow-y: auto;
  box-sizing: border-box;
  animation: popUp 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
}

@media (min-width: 500px) {
  .modal-card {
    padding: 2.25rem 2rem;
    border-radius: 20px;
  }
}

@keyframes popUp {
  from {
    transform: scale(0.92);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}

.success-icon-wrap {
  width: 54px;
  height: 54px;
  border-radius: 50%;
  background: #fff1eb;
  border: 2px solid #fed7aa;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 1rem;
}

@media (min-width: 500px) {
  .success-icon-wrap {
    width: 68px;
    height: 68px;
    margin-bottom: 1.25rem;
  }
}

.success-icon {
  font-size: 1.75rem;
}

@media (min-width: 500px) {
  .success-icon {
    font-size: 2.25rem;
  }
}

.success-title {
  font-size: 1.25rem;
  font-weight: 800;
  color: #1e293b;
  margin: 0 0 0.35rem;
  line-height: 1.25;
}

@media (min-width: 500px) {
  .success-title {
    font-size: 1.45rem;
    margin-bottom: 0.5rem;
  }
}

.success-desc {
  font-size: 0.8rem;
  color: #64748b;
  margin: 0 0 1.15rem;
  line-height: 1.45;
}

@media (min-width: 500px) {
  .success-desc {
    font-size: 0.875rem;
    margin-bottom: 1.5rem;
  }
}

.order-receipt-box {
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  padding: 0.95rem;
  text-align: left;
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
  margin-bottom: 1.25rem;
  width: 100%;
  box-sizing: border-box;
}

@media (min-width: 500px) {
  .order-receipt-box {
    border-radius: 14px;
    padding: 1.25rem;
    gap: 0.7rem;
    margin-bottom: 1.5rem;
  }
}

.receipt-row {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  font-size: 0.8rem;
  gap: 0.6rem;
  width: 100%;
}

@media (min-width: 500px) {
  .receipt-row {
    font-size: 0.825rem;
    gap: 0.75rem;
  }
}

.receipt-row .label {
  color: #64748b;
  flex-shrink: 0;
}

.receipt-row .value {
  color: #1e293b;
  font-weight: 600;
  text-align: right;
  word-break: break-word;
}

.order-id {
  color: #ff5a1f;
  font-family: monospace;
  font-size: 0.85rem;
}

.address-val {
  max-width: 170px;
  font-size: 0.75rem;
  line-height: 1.35;
}

@media (min-width: 500px) {
  .address-val {
    max-width: 220px;
    font-size: 0.775rem;
  }
}

.arrival-val {
  color: #065f46;
  font-weight: 700;
}

.receipt-divider {
  height: 1px;
  background-color: #e2e8f0;
  border-top: 1px dashed #e2e8f0;
  margin: 0.25rem 0;
}

.total-highlight {
  font-size: 0.9rem;
  align-items: center;
  padding-top: 0.2rem;
}

@media (min-width: 500px) {
  .total-highlight {
    font-size: 0.95rem;
    padding-top: 0.25rem;
  }
}

.total-highlight .label {
  font-weight: 700;
  color: #1e293b;
}

.total-highlight .price {
  font-size: 1.15rem;
  font-weight: 800;
  color: #ff5a1f;
}

@media (min-width: 500px) {
  .total-highlight .price {
    font-size: 1.25rem;
  }
}

.btn-primary-finish {
  width: 100%;
  background: linear-gradient(135deg, #ff6b35 0%, #ff3d00 100%);
  color: #ffffff;
  border: none;
  border-radius: 9999px;
  padding: 0.85rem;
  font-family: inherit;
  font-size: 0.9rem;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 4px 14px rgba(255, 90, 31, 0.3);
  transition: all 0.25s ease;
}

@media (min-width: 500px) {
  .btn-primary-finish {
    padding: 0.9rem;
    font-size: 0.95rem;
  }
}

.btn-primary-finish:hover {
  background: #e04810;
  transform: translateY(-2px);
  box-shadow: 0 8px 22px rgba(255, 90, 31, 0.4);
}
</style>
