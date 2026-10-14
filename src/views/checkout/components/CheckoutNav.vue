<script setup lang="ts">
import { useRouter, RouterLink } from "vue-router";

const router = useRouter();
</script>

<template>
  <div class="checkout-header-wrapper">
    <!-- Top Navigation & Breadcrumb -->
    <nav class="checkout-nav" aria-label="Breadcrumb">
      <button class="btn-back" @click="router.push('/cart')" title="Kembali ke Keranjang">
        <span class="back-arrow">←</span>
        <span class="btn-back-text">Kembali ke Keranjang</span>
        <span class="btn-back-text-mobile">Keranjang</span>
      </button>

      <div class="breadcrumb">
        <RouterLink to="/menu" class="breadcrumb-link">Menu</RouterLink>
        <span class="breadcrumb-separator">/</span>
        <RouterLink to="/cart" class="breadcrumb-link">Keranjang</RouterLink>
        <span class="breadcrumb-separator">/</span>
        <span class="breadcrumb-current">Checkout</span>
      </div>
    </nav>

    <!-- Page Title Header -->
    <header class="checkout-title-bar">
      <h1 class="page-title">Selesaikan Pesanan</h1>
      <span class="security-badge">
        <span class="lock-icon">🔒</span>
        <span>Pembayaran Aman & Terverifikasi</span>
      </span>
    </header>
  </div>
</template>

<style scoped>
.checkout-header-wrapper {
  margin-bottom: 1.5rem;
  width: 100%;
}

@media (min-width: 600px) {
  .checkout-header-wrapper {
    margin-bottom: 2rem;
  }
}

.checkout-nav {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1.25rem;
  flex-wrap: wrap;
  width: 100%;
}

@media (max-width: 500px) {
  .checkout-nav {
    gap: 0.65rem;
    margin-bottom: 1rem;
  }
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

.btn-back-text-mobile {
  display: none;
}

@media (max-width: 500px) {
  .btn-back {
    padding: 0.4rem 0.8rem;
    font-size: 0.8rem;
  }
  .btn-back-text {
    display: none;
  }
  .btn-back-text-mobile {
    display: inline;
  }
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
  font-size: 0.825rem;
  color: #64748b;
  min-width: 0;
  overflow: hidden;
}

.breadcrumb-link {
  color: #64748b;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s;
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
}

.checkout-title-bar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 0.75rem;
  width: 100%;
}

.page-title {
  font-size: 1.45rem;
  font-weight: 800;
  color: #1e293b;
  margin: 0;
  word-break: break-word;
}

@media (min-width: 600px) {
  .page-title {
    font-size: 1.85rem;
  }
}

.security-badge {
  display: inline-flex;
  align-items: center;
  gap: 0.35rem;
  background-color: #d1fae5;
  color: #065f46;
  font-size: 0.725rem;
  font-weight: 700;
  padding: 0.3rem 0.75rem;
  border-radius: 9999px;
  flex-shrink: 0;
}

@media (min-width: 600px) {
  .security-badge {
    font-size: 0.775rem;
    padding: 0.35rem 0.85rem;
  }
}

.lock-icon {
  font-size: 0.85rem;
}
</style>
