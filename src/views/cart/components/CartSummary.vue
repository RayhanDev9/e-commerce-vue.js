<script setup lang="ts">
import { ref } from "vue";
import useFormatCurrency from "@/composables/useCurrency";

const props = defineProps<{
  totalItems: number;
  totalPrice: number;
  totalDiscount: number;
  discountCodeApplied: boolean;
}>();

const emit = defineEmits<{
  (e: "applyVoucher", code: string): void;
  (e: "checkout"): void;
}>();

const voucherCode = ref("");

const handleApply = () => {
  if (voucherCode.value.trim()) {
    emit("applyVoucher", voucherCode.value.trim());
  }
};
</script>

<template>
  <aside class="order-summary-column">
    <div class="summary-card">
      <h2 class="summary-title">Ringkasan Pesanan</h2>

      <!-- Voucher Code Box -->
      <div class="voucher-box">
        <input
          v-model="voucherCode"
          type="text"
          class="voucher-input"
          placeholder="Kode Promo (MAKANENAK)"
          :disabled="discountCodeApplied"
          @keyup.enter="handleApply"
        />
        <button
          type="button"
          class="btn-voucher"
          :disabled="discountCodeApplied || !voucherCode"
          @click="handleApply"
        >
          {{ discountCodeApplied ? 'Terpasang' : 'Terapkan' }}
        </button>
      </div>

      <!-- Detail Biaya -->
      <div class="summary-breakdown">
        <div class="breakdown-row">
          <span class="label">Total Porsi</span>
          <span class="value">{{ totalItems }} menu</span>
        </div>

        <div class="breakdown-row">
          <span class="label">Subtotal Harga</span>
          <span class="value">{{ useFormatCurrency(totalPrice + totalDiscount) }}</span>
        </div>

        <div v-if="totalDiscount > 0" class="breakdown-row promo-row">
          <span class="label">Diskon Menu</span>
          <span class="value">-{{ useFormatCurrency(totalDiscount) }}</span>
        </div>

        <div v-if="discountCodeApplied" class="breakdown-row promo-row">
          <span class="label">Voucher (MAKANENAK)</span>
          <span class="value">-{{ useFormatCurrency(10000) }}</span>
        </div>

        <div class="breakdown-row">
          <span class="label">Biaya Pengiriman</span>
          <span class="value free-badge">GRATIS</span>
        </div>
      </div>

      <div class="summary-divider"></div>

      <!-- Total Akhir -->
      <div class="total-row">
        <div class="total-text-group">
          <span class="total-label">Total Pembayaran</span>
          <span class="tax-info">Sudah termasuk pajak & layanan</span>
        </div>
        <strong class="total-amount">
          {{ useFormatCurrency(discountCodeApplied ? Math.max(0, totalPrice - 10000) : totalPrice) }}
        </strong>
      </div>

      <!-- Tombol Checkout -->
      <button
        type="button"
        class="btn-checkout"
        @click="emit('checkout')"
      >
        <span>Lanjut ke Pembayaran</span>
        <span class="checkout-arrow">→</span>
      </button>

      <!-- Secure Guarantee Note -->
      <div class="secure-note">
        <span>🔒 Pembayaran aman & diproses otomatis</span>
      </div>
    </div>
  </aside>
</template>

<style scoped>
.order-summary-column {
  position: sticky;
  top: 1.5rem;
  width: 100%;
}

.summary-card {
  background-color: #ffffff;
  border-radius: 16px;
  border: 1px solid #e2e8f0;
  padding: 1.75rem;
  box-shadow: 0 4px 20px rgba(15, 23, 42, 0.06);
  width: 100%;
  box-sizing: border-box;
}

@media (max-width: 480px) {
  .summary-card {
    padding: 1.15rem 1rem;
    border-radius: 14px;
  }
}

@media (max-width: 380px) {
  .summary-card {
    padding: 1rem 0.8rem;
  }
}

.summary-title {
  font-size: 1.15rem;
  font-weight: 800;
  color: #1e293b;
  margin: 0 0 1rem;
}

@media (min-width: 600px) {
  .summary-title {
    font-size: 1.25rem;
    margin-bottom: 1.25rem;
  }
}

.voucher-box {
  display: flex;
  gap: 0.45rem;
  margin-bottom: 1.25rem;
  width: 100%;
}

.voucher-input {
  flex: 1;
  min-width: 0;
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 9999px;
  padding: 0.6rem 0.85rem;
  font-family: inherit;
  font-size: 0.8rem;
  outline: none;
  transition: border-color 0.2s;
  box-sizing: border-box;
}

@media (max-width: 380px) {
  .voucher-input {
    padding: 0.55rem 0.7rem;
    font-size: 0.75rem;
  }
}

.voucher-input:focus {
  border-color: #ff5a1f;
}

.btn-voucher {
  background-color: #1e293b;
  color: #ffffff;
  border: none;
  border-radius: 9999px;
  padding: 0.6rem 0.95rem;
  font-family: inherit;
  font-size: 0.8rem;
  font-weight: 700;
  cursor: pointer;
  flex-shrink: 0;
  transition: all 0.2s ease;
}

@media (max-width: 380px) {
  .btn-voucher {
    padding: 0.55rem 0.75rem;
    font-size: 0.75rem;
  }
}

.btn-voucher:hover:not(:disabled) {
  background-color: #ff5a1f;
}

.btn-voucher:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.summary-breakdown {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  width: 100%;
}

.breakdown-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 0.825rem;
  gap: 0.5rem;
  width: 100%;
}

@media (min-width: 600px) {
  .breakdown-row {
    font-size: 0.875rem;
    gap: 0.75rem;
  }
}

.breakdown-row .label {
  color: #64748b;
  min-width: 0;
  word-break: break-word;
}

.breakdown-row .value {
  font-weight: 600;
  color: #1e293b;
  flex-shrink: 0;
  white-space: nowrap;
}

.promo-row .label,
.promo-row .value {
  color: #10b981;
  font-weight: 700;
}

.free-badge {
  color: #10b981;
  font-weight: 800;
}

.summary-divider {
  height: 1px;
  background-color: #e2e8f0;
  margin: 1rem 0;
  border-top: 1px dashed #e2e8f0;
}

@media (min-width: 600px) {
  .summary-divider {
    margin: 1.25rem 0;
  }
}

.total-row {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 0.5rem;
  margin-bottom: 1.25rem;
  width: 100%;
}

@media (min-width: 600px) {
  .total-row {
    margin-bottom: 1.5rem;
  }
}

.total-text-group {
  display: flex;
  flex-direction: column;
  gap: 0.15rem;
}

.total-label {
  font-size: 0.9rem;
  font-weight: 700;
  color: #1e293b;
}

.tax-info {
  font-size: 0.7rem;
  color: #94a3b8;
}

.total-amount {
  font-size: 1.45rem;
  font-weight: 800;
  color: #ff5a1f;
  line-height: 1;
  white-space: nowrap;
  flex-shrink: 0;
}

@media (min-width: 480px) {
  .total-amount {
    font-size: 1.75rem;
  }
}

@media (max-width: 360px) {
  .total-amount {
    font-size: 1.3rem;
  }
}

.btn-checkout {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  background: linear-gradient(135deg, #ff6b35 0%, #ff3d00 100%);
  color: #ffffff;
  border: none;
  border-radius: 9999px;
  padding: 0.85rem;
  font-family: inherit;
  font-size: 0.95rem;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 6px 18px rgba(255, 90, 31, 0.3);
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  box-sizing: border-box;
}

@media (min-width: 600px) {
  .btn-checkout {
    padding: 0.95rem;
    font-size: 1rem;
    gap: 0.6rem;
  }
}

.btn-checkout:hover {
  background: #e04810;
  transform: translateY(-2px);
  box-shadow: 0 10px 25px rgba(255, 90, 31, 0.38);
}

.btn-checkout:active {
  transform: scale(0.98);
}

.checkout-arrow {
  font-size: 1.1rem;
  line-height: 1;
}

.secure-note {
  text-align: center;
  margin-top: 0.85rem;
  font-size: 0.725rem;
  color: #94a3b8;
}
</style>
