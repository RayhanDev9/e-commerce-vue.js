<script setup lang="ts">
export interface ShippingOption {
  id: string;
  name: string;
  estimate: string;
  price: number;
  badge?: string;
}

export interface PaymentOption {
  id: string;
  name: string;
  description: string;
  icon: string;
}

defineProps<{
  shippingMethod: string;
  paymentMethod: string;
}>();

const emit = defineEmits<{
  (e: "update:shippingMethod", val: string): void;
  (e: "update:paymentMethod", val: string): void;
}>();

const shippingOptions: ShippingOption[] = [
  {
    id: "regular",
    name: "Pengiriman Reguler",
    estimate: "1 - 2 Jam (Estimasi Siap)",
    price: 0,
    badge: "GRATIS",
  },
  {
    id: "instant",
    name: "Pengiriman Instan Cepat",
    estimate: "30 - 45 Menit Sampai",
    price: 10000,
    badge: "Prioritas",
  },
];

const paymentOptions: PaymentOption[] = [
  {
    id: "qris",
    name: "QRIS / E-Wallet Instant",
    description: "Scan via GoPay, OVO, Dana, ShopeePay & BCA Mobile",
    icon: "📱",
  },
  {
    id: "bca_va",
    name: "BCA Virtual Account",
    description: "Konfirmasi otomatis tanpa perlu upload struk transfer",
    icon: "🏦",
  },
  {
    id: "cod",
    name: "Bayar di Tempat (COD)",
    description: "Bayar uang tunai langsung ke kurir saat makanan tiba",
    icon: "💵",
  },
];
</script>

<template>
  <div class="checkout-card shipping-payment-card">
    <!-- 1. METODE PENGIRIMAN -->
    <div class="section-block">
      <div class="card-header">
        <span class="step-num">2</span>
        <div class="header-text-group">
          <h2 class="card-title">Metode Pengiriman Makanan</h2>
          <p class="card-subtitle">Pilih kecepatan antar kurir makanan Anda</p>
        </div>
      </div>

      <div class="options-grid">
        <label
          v-for="opt in shippingOptions"
          :key="opt.id"
          class="option-pill"
          :class="{ active: shippingMethod === opt.id }"
        >
          <input
            type="radio"
            name="shipping_method"
            :value="opt.id"
            :checked="shippingMethod === opt.id"
            class="radio-input"
            @change="emit('update:shippingMethod', opt.id)"
          />
          <div class="option-content">
            <div class="opt-top-row">
              <span class="opt-name">{{ opt.name }}</span>
              <span class="opt-badge" :class="{ free: opt.price === 0 }">{{ opt.badge }}</span>
            </div>
            <span class="opt-estimate">⏱️ {{ opt.estimate }}</span>
            <span class="opt-price">
              {{ opt.price === 0 ? 'Rp 0 (Promo Free Delivery)' : 'Rp 10.000' }}
            </span>
          </div>
        </label>
      </div>
    </div>

    <!-- 2. METODE PEMBAYARAN -->
    <div class="section-block mt-block">
      <div class="card-header">
        <span class="step-num">3</span>
        <div class="header-text-group">
          <h2 class="card-title">Metode Pembayaran</h2>
          <p class="card-subtitle">Pilih cara pembayaran yang paling nyaman untuk Anda</p>
        </div>
      </div>

      <div class="options-vertical">
        <label
          v-for="pay in paymentOptions"
          :key="pay.id"
          class="payment-option-row"
          :class="{ active: paymentMethod === pay.id }"
        >
          <input
            type="radio"
            name="payment_method"
            :value="pay.id"
            :checked="paymentMethod === pay.id"
            class="radio-input"
            @change="emit('update:paymentMethod', pay.id)"
          />
          <span class="pay-icon">{{ pay.icon }}</span>
          <div class="pay-info">
            <strong class="pay-name">{{ pay.name }}</strong>
            <span class="pay-desc">{{ pay.description }}</span>
          </div>
          <span class="custom-radio-check"></span>
        </label>
      </div>
    </div>
  </div>
</template>

<style scoped>
.checkout-card {
  background-color: #ffffff;
  border-radius: 16px;
  border: 1px solid #e2e8f0;
  padding: 1.75rem;
  box-shadow: 0 4px 20px rgba(15, 23, 42, 0.06);
  width: 100%;
  box-sizing: border-box;
}

@media (max-width: 500px) {
  .checkout-card {
    padding: 1.15rem 1rem;
    border-radius: 14px;
  }
}

.mt-block {
  margin-top: 1.5rem;
  padding-top: 1.35rem;
  border-top: 1px dashed #e2e8f0;
}

@media (min-width: 600px) {
  .mt-block {
    margin-top: 2rem;
    padding-top: 1.75rem;
  }
}

.card-header {
  display: flex;
  align-items: flex-start;
  gap: 0.85rem;
  margin-bottom: 1.15rem;
  width: 100%;
}

.header-text-group {
  flex: 1;
  min-width: 0;
}

.step-num {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: linear-gradient(135deg, #ff6b35 0%, #ff3d00 100%);
  color: #ffffff;
  font-weight: 800;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.95rem;
  flex-shrink: 0;
}

@media (max-width: 500px) {
  .step-num {
    width: 28px;
    height: 28px;
    font-size: 0.85rem;
  }
}

.card-title {
  font-size: 1.15rem;
  font-weight: 800;
  color: #1e293b;
  margin: 0 0 0.2rem;
  word-break: break-word;
}

@media (max-width: 500px) {
  .card-title {
    font-size: 1.05rem;
    line-height: 1.3;
  }
}

.card-subtitle {
  font-size: 0.8rem;
  color: #64748b;
  margin: 0;
  line-height: 1.4;
}

@media (max-width: 500px) {
  .card-subtitle {
    font-size: 0.75rem;
  }
}

/* SHIPPING OPTIONS GRID */
.options-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 0.85rem;
  width: 100%;
}

@media (min-width: 600px) {
  .options-grid {
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }
}

.option-pill {
  position: relative;
  display: flex;
  padding: 0.9rem 1rem;
  border: 2px solid #e2e8f0;
  border-radius: 12px;
  cursor: pointer;
  background-color: #f8fafc;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  width: 100%;
  box-sizing: border-box;
}

@media (min-width: 600px) {
  .option-pill {
    padding: 1.1rem;
    border-radius: 14px;
  }
}

.option-pill:hover {
  border-color: #fed7aa;
  background-color: #ffffff;
}

.option-pill.active {
  border-color: #ff5a1f;
  background-color: #fff1eb;
  box-shadow: 0 4px 14px rgba(255, 90, 31, 0.12);
}

.radio-input {
  position: absolute;
  opacity: 0;
  pointer-events: none;
}

.option-content {
  display: flex;
  flex-direction: column;
  gap: 0.3rem;
  width: 100%;
  min-width: 0;
}

.opt-top-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 0.5rem;
}

.opt-name {
  font-size: 0.875rem;
  font-weight: 700;
  color: #1e293b;
}

.opt-badge {
  font-size: 0.675rem;
  font-weight: 800;
  padding: 0.15rem 0.45rem;
  border-radius: 9999px;
  background-color: #ffedd5;
  color: #c2410c;
  flex-shrink: 0;
}

.opt-badge.free {
  background-color: #d1fae5;
  color: #065f46;
}

.opt-estimate {
  font-size: 0.75rem;
  color: #64748b;
}

.opt-price {
  font-size: 0.825rem;
  font-weight: 800;
  color: #ff5a1f;
  margin-top: 0.15rem;
  word-break: break-word;
}

/* PAYMENT OPTIONS */
.options-vertical {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  width: 100%;
}

.payment-option-row {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.85rem 1rem;
  border: 2px solid #e2e8f0;
  border-radius: 12px;
  background-color: #f8fafc;
  cursor: pointer;
  transition: all 0.2s ease;
  width: 100%;
  box-sizing: border-box;
}

@media (min-width: 600px) {
  .payment-option-row {
    padding: 1rem 1.25rem;
    gap: 1rem;
    border-radius: 14px;
  }
}

.payment-option-row:hover {
  border-color: #fed7aa;
  background-color: #ffffff;
}

.payment-option-row.active {
  border-color: #ff5a1f;
  background-color: #fff1eb;
  box-shadow: 0 4px 14px rgba(255, 90, 31, 0.12);
}

.pay-icon {
  font-size: 1.35rem;
  flex-shrink: 0;
}

@media (min-width: 600px) {
  .pay-icon {
    font-size: 1.5rem;
  }
}

.pay-info {
  flex: 1;
  min-width: 0;
  display: flex;
  flex-direction: column;
  gap: 0.15rem;
}

.pay-name {
  font-size: 0.875rem;
  color: #1e293b;
  word-break: break-word;
}

.pay-desc {
  font-size: 0.725rem;
  color: #64748b;
  line-height: 1.3;
}

.custom-radio-check {
  width: 18px;
  height: 18px;
  border-radius: 50%;
  border: 2px solid #cbd5e1;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  transition: all 0.2s;
}

@media (min-width: 600px) {
  .custom-radio-check {
    width: 20px;
    height: 20px;
  }
}

.payment-option-row.active .custom-radio-check {
  border-color: #ff5a1f;
  background-color: #ff5a1f;
  box-shadow: inset 0 0 0 3px #ffffff;
}
</style>
