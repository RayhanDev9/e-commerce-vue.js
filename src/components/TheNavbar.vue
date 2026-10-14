<script setup lang="ts">
import { ref, watch } from "vue";
import { RouterLink, useRoute, useRouter } from "vue-router";
import { useCartStore } from "@/stores/useCart";
import { useAuthStore } from "@/stores/useAuth";
import {
  UtensilsCrossed,
  Utensils,
  Settings,
  ShoppingCart,
  LogIn,
  LogOut,
  Shield,
  User as UserIcon,
  Menu as MenuIcon,
  X as XIcon,
} from "lucide-vue-next";

const route = useRoute();
const router = useRouter();
const cartStore = useCartStore();
const authStore = useAuthStore();

const isMobileMenuOpen = ref(false);

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
};

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false;
};

// Tutup menu mobile jika rute berganti
watch(
  () => route.path,
  () => {
    isMobileMenuOpen.value = false;
  }
);

const handleLogout = () => {
  closeMobileMenu();
  authStore.logout();
  router.push("/login");
};
</script>

<template>
  <header class="app-navbar">
    <div class="navbar-container">
      <!-- 1. Brand Logo -->
      <RouterLink to="/menu" class="brand-link" @click="closeMobileMenu">
        <div class="brand-icon-box">
          <UtensilsCrossed :size="22" class="brand-icon" />
        </div>
        <div class="brand-text">
          <span class="brand-name">Dapur<span class="brand-highlight">Rasa</span></span>
          <span class="brand-badge">Food Store</span>
        </div>
      </RouterLink>

      <!-- 2. Mobile Actions (< md: Cart Quick Button + Hamburger Toggle) -->
      <div class="mobile-actions">
        <!-- Quick Cart Shortcut on Mobile -->
        <RouterLink
          to="/cart"
          class="mobile-cart-btn"
          :class="{ active: route.path === '/cart' }"
          title="Keranjang Belanja"
          @click="closeMobileMenu"
        >
          <ShoppingCart :size="20" class="cart-svg-icon" />
          <span v-if="cartStore.totalItems > 0" class="cart-counter-badge">
            {{ cartStore.totalItems > 99 ? '99+' : cartStore.totalItems }}
          </span>
        </RouterLink>

        <!-- Hamburger Toggle Button -->
        <button
          type="button"
          class="btn-hamburger"
          :class="{ 'is-active': isMobileMenuOpen }"
          :aria-expanded="isMobileMenuOpen"
          aria-label="Menu navigasi"
          @click="toggleMobileMenu"
        >
          <XIcon v-if="isMobileMenuOpen" :size="22" class="hamburger-icon" />
          <MenuIcon v-else :size="22" class="hamburger-icon" />
        </button>
      </div>

      <!-- 3. Desktop Nav Action Links (>= md) -->
      <nav class="nav-menu desktop-nav">
        <!-- Link Menu -->
        <RouterLink
          to="/menu"
          class="nav-item"
          :class="{ active: route.path.startsWith('/menu') }"
        >
          <Utensils :size="18" class="nav-icon" />
          <span>Menu</span>
        </RouterLink>

        <!-- Tombol Admin (HANYA MUNCUL JIKA USER ADALAH ADMIN) -->
        <RouterLink
          v-if="authStore.isAdmin"
          to="/admin"
          class="nav-item nav-admin-btn"
          :class="{ active: route.path.startsWith('/admin') }"
          title="Manajemen Produk Makanan"
        >
          <Settings :size="18" class="nav-icon" />
          <span>Admin</span>
        </RouterLink>

        <!-- Icon Keranjang Belanja -->
        <RouterLink
          to="/cart"
          class="nav-item nav-cart-btn"
          :class="{ active: route.path === '/cart' }"
          title="Keranjang Belanja"
        >
          <!-- Floating Badge Counter -->
          <span v-if="cartStore.totalItems > 0" class="cart-counter-badge">
            {{ cartStore.totalItems > 99 ? '99+' : cartStore.totalItems }}
          </span>
          <div class="cart-icon-wrapper">
            <ShoppingCart :size="19" class="cart-svg-icon" />
          </div>
          <span class="cart-text">Keranjang</span>
        </RouterLink>

        <div class="nav-divider"></div>

        <!-- 4. User Authentication Controls (Desktop) -->
        <!-- Jika Belum Login -->
        <RouterLink
          v-if="!authStore.isAuthenticated"
          to="/login"
          class="nav-item nav-login-btn"
          title="Masuk ke Akun"
        >
          <LogIn :size="17" />
          <span>Masuk</span>
        </RouterLink>

        <!-- Jika Sudah Login -->
        <div v-else class="user-profile-badge">
          <div class="user-avatar-wrap">
            <img
              v-if="authStore.currentUser?.avatar"
              :src="authStore.currentUser.avatar"
              :alt="authStore.userName"
              class="user-avatar-img"
            />
            <UserIcon v-else :size="16" />
          </div>

          <div class="user-info-text">
            <span class="user-name">{{ authStore.userName }}</span>
            <span
              class="user-role-pill"
              :class="authStore.isAdmin ? 'role-admin' : 'role-customer'"
            >
              <Shield v-if="authStore.isAdmin" :size="11" />
              <span>{{ authStore.isAdmin ? 'Admin' : 'Pelanggan' }}</span>
            </span>
          </div>

          <!-- Tombol Logout -->
          <button
            type="button"
            class="btn-logout"
            title="Keluar dari Akun"
            @click="handleLogout"
          >
            <LogOut :size="16" />
          </button>
        </div>
      </nav>
    </div>

    <!-- 5. Mobile Backdrop Overlay (< md) -->
    <Transition name="fade">
      <div
        v-if="isMobileMenuOpen"
        class="mobile-backdrop"
        @click="closeMobileMenu"
      ></div>
    </Transition>

    <!-- 6. Mobile Menu Drawer (< md) -->
    <Transition name="slide-down">
      <nav v-if="isMobileMenuOpen" class="mobile-drawer">
        <!-- Profile info jika login -->
        <div v-if="authStore.isAuthenticated" class="mobile-profile-card">
          <div class="mobile-avatar-wrap">
            <img
              v-if="authStore.currentUser?.avatar"
              :src="authStore.currentUser.avatar"
              :alt="authStore.userName"
              class="mobile-avatar-img"
            />
            <UserIcon v-else :size="20" />
          </div>
          <div class="mobile-user-details">
            <span class="mobile-user-name">{{ authStore.userName }}</span>
            <span
              class="user-role-pill"
              :class="authStore.isAdmin ? 'role-admin' : 'role-customer'"
            >
              <Shield v-if="authStore.isAdmin" :size="11" />
              <span>{{ authStore.isAdmin ? 'Admin' : 'Pelanggan' }}</span>
            </span>
          </div>
        </div>

        <!-- Links List -->
        <div class="mobile-nav-links">
          <RouterLink
            to="/menu"
            class="mobile-nav-item"
            :class="{ active: route.path.startsWith('/menu') }"
            @click="closeMobileMenu"
          >
            <div class="mobile-nav-icon-box">
              <Utensils :size="18" />
            </div>
            <span class="mobile-nav-label">Menu Makanan</span>
          </RouterLink>

          <RouterLink
            v-if="authStore.isAdmin"
            to="/admin"
            class="mobile-nav-item"
            :class="{ active: route.path.startsWith('/admin') }"
            @click="closeMobileMenu"
          >
            <div class="mobile-nav-icon-box">
              <Settings :size="18" />
            </div>
            <span class="mobile-nav-label">Kelola Menu (Admin)</span>
            <span class="mobile-tag-admin">Admin</span>
          </RouterLink>

          <RouterLink
            to="/cart"
            class="mobile-nav-item"
            :class="{ active: route.path === '/cart' }"
            @click="closeMobileMenu"
          >
            <div class="mobile-nav-icon-box">
              <ShoppingCart :size="18" />
            </div>
            <span class="mobile-nav-label">Keranjang Belanja</span>
            <span v-if="cartStore.totalItems > 0" class="mobile-tag-cart">
              {{ cartStore.totalItems }} item
            </span>
          </RouterLink>
        </div>

        <!-- Auth Buttons -->
        <div class="mobile-auth-footer">
          <RouterLink
            v-if="!authStore.isAuthenticated"
            to="/login"
            class="mobile-btn-login"
            @click="closeMobileMenu"
          >
            <LogIn :size="18" />
            <span>Masuk ke Akun</span>
          </RouterLink>

          <button
            v-else
            type="button"
            class="mobile-btn-logout"
            @click="handleLogout"
          >
            <LogOut :size="18" />
            <span>Keluar dari Akun</span>
          </button>
        </div>
      </nav>
    </Transition>
  </header>
</template>

<style scoped>
/* ==========================================================================
   THEME NAVBAR WITH RESPONSIVE HAMBURGER (< md)
   ========================================================================== */

.app-navbar {
  position: sticky;
  top: 0;
  z-index: 100;
  background-color: rgba(255, 255, 255, 0.96);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border-bottom: 1px solid #e2e8f0;
  box-shadow: 0 4px 20px rgba(15, 23, 42, 0.04);
  font-family: "Plus Jakarta Sans", system-ui, -apple-system, sans-serif;
  width: 100%;
}

.navbar-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0.85rem 1.25rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

/* BRAND */
.brand-link {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  text-decoration: none;
  transition: transform 0.2s ease;
  user-select: none;
}

.brand-link:hover {
  transform: scale(1.02);
}

.brand-icon-box {
  width: 40px;
  height: 40px;
  border-radius: 12px;
  background: linear-gradient(135deg, #ff5a1f 0%, #ff3d00 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 10px rgba(255, 90, 31, 0.28);
  flex-shrink: 0;
}

.brand-icon {
  color: #ffffff;
}

.brand-text {
  display: flex;
  flex-direction: column;
}

.brand-name {
  font-size: 1.25rem;
  font-weight: 800;
  color: #1e293b;
  line-height: 1.1;
  letter-spacing: -0.02em;
}

.brand-highlight {
  color: #ff5a1f;
}

.brand-badge {
  font-size: 0.65rem;
  font-weight: 700;
  color: #94a3b8;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}

/* DESKTOP NAV LINKS (>= md) */
.desktop-nav {
  display: none;
  align-items: center;
  gap: 0.65rem;
}

@media (min-width: 768px) {
  .desktop-nav {
    display: flex;
  }
}

.nav-item {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 600;
  color: #475569;
  padding: 0.55rem 0.95rem;
  border-radius: 9999px;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.nav-item:hover {
  background-color: #fff1eb;
  color: #ff5a1f;
}

.nav-item.active {
  background-color: #fff1eb;
  color: #ff5a1f;
  font-weight: 700;
}

/* TOMBOL ADMIN */
.nav-admin-btn {
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  color: #334155;
}

.nav-admin-btn:hover {
  background-color: #f1f5f9;
  color: #0f172a;
  border-color: #cbd5e1;
}

.nav-admin-btn.active {
  background-color: #1e293b;
  color: #ffffff;
  border-color: #1e293b;
}

.nav-admin-btn.active .nav-icon {
  color: #ffffff;
}

/* TOMBOL KERANJANG */
.nav-cart-btn {
  position: relative;
  background: linear-gradient(135deg, #ff6b35 0%, #ff3d00 100%);
  color: #ffffff;
  box-shadow: 0 4px 12px rgba(255, 90, 31, 0.25);
}

.nav-cart-btn:hover {
  background: #e04810;
  color: #ffffff;
  transform: translateY(-1px);
  box-shadow: 0 6px 16px rgba(255, 90, 31, 0.35);
}

.nav-cart-btn.active {
  background: #e04810;
  color: #ffffff;
}

.cart-icon-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}

.cart-svg-icon {
  stroke-width: 2.2;
}

.cart-counter-badge {
  position: absolute;
  top: -8px;
  right: -10px;
  background-color: #ffffff;
  color: #ff5a1f;
  font-size: 0.675rem;
  font-weight: 800;
  min-width: 18px;
  height: 18px;
  border-radius: 9999px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 4px;
  border: 2px solid #ff5a1f;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
  animation: popIn 0.25s cubic-bezier(0.34, 1.56, 0.64, 1);
  z-index: 2;
}

@keyframes popIn {
  from {
    transform: scale(0);
  }
  to {
    transform: scale(1);
  }
}

.nav-divider {
  width: 1px;
  height: 24px;
  background-color: #e2e8f0;
  margin: 0 0.25rem;
}

/* TOMBOL MASUK */
.nav-login-btn {
  background-color: #f1f5f9;
  border: 1px solid #cbd5e1;
  color: #334155;
  font-weight: 700;
}

.nav-login-btn:hover {
  background-color: #e2e8f0;
  color: #0f172a;
}

/* USER PROFILE BADGE */
.user-profile-badge {
  display: flex;
  align-items: center;
  gap: 0.65rem;
  padding: 0.35rem 0.65rem 0.35rem 0.4rem;
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 9999px;
}

.user-avatar-wrap {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  overflow: hidden;
  background-color: #e2e8f0;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #64748b;
  flex-shrink: 0;
}

.user-avatar-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.user-info-text {
  display: flex;
  flex-direction: column;
  line-height: 1.15;
}

.user-name {
  font-size: 0.8rem;
  font-weight: 700;
  color: #1e293b;
  max-width: 95px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.user-role-pill {
  display: inline-flex;
  align-items: center;
  gap: 0.2rem;
  font-size: 0.65rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 0.04em;
  padding: 0.1rem 0.35rem;
  border-radius: 4px;
  width: fit-content;
}

.role-admin {
  background-color: #fff1eb;
  color: #c2410c;
}

.role-customer {
  background-color: #eff6ff;
  color: #1d4ed8;
}

.btn-logout {
  background: none;
  border: none;
  color: #94a3b8;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0.35rem;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.15s ease;
}

.btn-logout:hover {
  background-color: #fee2e2;
  color: #dc2626;
}

/* ==========================================================================
   MOBILE BAR & HAMBURGER (< md, < 768px)
   ========================================================================== */

.mobile-actions {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

@media (min-width: 768px) {
  .mobile-actions {
    display: none;
  }
}

.mobile-cart-btn {
  position: relative;
  width: 40px;
  height: 40px;
  border-radius: 12px;
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  color: #334155;
  display: flex;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  transition: all 0.2s ease;
}

.mobile-cart-btn:hover,
.mobile-cart-btn.active {
  background-color: #fff1eb;
  color: #ff5a1f;
  border-color: #ffccbc;
}

.mobile-cart-btn .cart-counter-badge {
  top: -6px;
  right: -6px;
}

.btn-hamburger {
  width: 40px;
  height: 40px;
  border-radius: 12px;
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  color: #1e293b;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 1px 3px rgba(15, 23, 42, 0.05);
}

.btn-hamburger:hover {
  background-color: #f1f5f9;
  border-color: #cbd5e1;
}

.btn-hamburger.is-active {
  background-color: #fff1eb;
  color: #ff5a1f;
  border-color: #ffccbc;
}

.hamburger-icon {
  stroke-width: 2.2;
}

/* MOBILE BACKDROP */
.mobile-backdrop {
  position: fixed;
  top: 61px;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(15, 23, 42, 0.45);
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
  z-index: 90;
}

@media (min-width: 768px) {
  .mobile-backdrop {
    display: none !important;
  }
}

/* MOBILE DRAWER / MENU */
.mobile-drawer {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background-color: #ffffff;
  border-bottom: 1px solid #e2e8f0;
  box-shadow: 0 12px 28px rgba(15, 23, 42, 0.12);
  padding: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  z-index: 95;
  max-height: calc(100vh - 70px);
  overflow-y: auto;
}

@media (min-width: 768px) {
  .mobile-drawer {
    display: none !important;
  }
}

/* PROFILE CARD IN MOBILE DRAWER */
.mobile-profile-card {
  display: flex;
  align-items: center;
  gap: 0.85rem;
  padding: 0.85rem 1rem;
  background-color: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 12px;
}

.mobile-avatar-wrap {
  width: 42px;
  height: 42px;
  border-radius: 50%;
  overflow: hidden;
  background-color: #e2e8f0;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #64748b;
  flex-shrink: 0;
}

.mobile-avatar-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.mobile-user-details {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
}

.mobile-user-name {
  font-size: 0.95rem;
  font-weight: 700;
  color: #1e293b;
}

/* MOBILE NAV LINKS */
.mobile-nav-links {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.mobile-nav-item {
  display: flex;
  align-items: center;
  gap: 0.85rem;
  padding: 0.75rem 1rem;
  border-radius: 12px;
  text-decoration: none;
  color: #334155;
  font-size: 0.95rem;
  font-weight: 600;
  transition: all 0.2s ease;
  background-color: #ffffff;
  border: 1px solid transparent;
}

.mobile-nav-item:hover {
  background-color: #f8fafc;
  color: #0f172a;
}

.mobile-nav-item.active {
  background-color: #fff1eb;
  color: #ff5a1f;
  font-weight: 700;
  border-color: #ffccbc;
}

.mobile-nav-icon-box {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background-color: #f1f5f9;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #475569;
  transition: all 0.2s ease;
}

.mobile-nav-item.active .mobile-nav-icon-box {
  background-color: #ff5a1f;
  color: #ffffff;
}

.mobile-nav-label {
  flex: 1;
}

.mobile-tag-admin {
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  background-color: #1e293b;
  color: #ffffff;
  padding: 0.2rem 0.5rem;
  border-radius: 6px;
}

.mobile-tag-cart {
  font-size: 0.75rem;
  font-weight: 800;
  background-color: #ff5a1f;
  color: #ffffff;
  padding: 0.2rem 0.55rem;
  border-radius: 9999px;
}

/* MOBILE AUTH FOOTER */
.mobile-auth-footer {
  padding-top: 0.75rem;
  border-top: 1px dashed #e2e8f0;
}

.mobile-btn-login {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  width: 100%;
  padding: 0.85rem;
  background: linear-gradient(135deg, #ff5a1f 0%, #ff3d00 100%);
  color: #ffffff;
  border-radius: 12px;
  font-weight: 700;
  font-size: 0.95rem;
  text-decoration: none;
  box-shadow: 0 4px 12px rgba(255, 90, 31, 0.25);
}

.mobile-btn-logout {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  width: 100%;
  padding: 0.85rem;
  background-color: #fee2e2;
  color: #dc2626;
  border: 1px solid #fecaca;
  border-radius: 12px;
  font-weight: 700;
  font-size: 0.95rem;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.mobile-btn-logout:hover {
  background-color: #fecaca;
}

/* ANIMATIONS */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.slide-down-enter-active,
.slide-down-leave-active {
  transition: all 0.25s cubic-bezier(0.16, 1, 0.3, 1);
}

.slide-down-enter-from,
.slide-down-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}
</style>
