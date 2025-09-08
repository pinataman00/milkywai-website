<template>
  <header class="header" :class="{ scrolled: isScrolled }">
    <nav class="nav">
      <a class="logo" href="#home">
        <img :src="logo" alt="milkywai logo" class="logo-img" />
      </a>
      
      <!-- 데스크톱 메뉴 -->
      <ul class="nav-links" :class="{ 'mobile-open': isMobileMenuOpen }">
        <li><a href="#home" @click="closeMobileMenu">Home</a></li>
        <li><a href="#services" @click="closeMobileMenu">Services</a></li>
        <li><a href="#solutions" @click="closeMobileMenu">Solutions</a></li>
        <li><a href="#about" @click="closeMobileMenu">About</a></li>
        <li><a href="#contact" @click="closeMobileMenu">Contact</a></li>
      </ul>
      
      <!-- 햄버거 메뉴 버튼 -->
      <div class="mobile-menu" :class="{ active: isMobileMenuOpen }" @click="toggleMobileMenu">
        <span></span>
        <span></span>
        <span></span>
      </div>
    </nav>
  </header>
</template>

<script>
import { ref } from 'vue'
import logo from '../assets/milkywai.svg'

export default {
  name: 'HeaderComponent',
  props: {
    isScrolled: {
      type: Boolean,
      default: false
    }
  },
  setup() {
    const isMobileMenuOpen = ref(false)

    const toggleMobileMenu = () => {
      isMobileMenuOpen.value = !isMobileMenuOpen.value
    }

    const closeMobileMenu = () => {
      isMobileMenuOpen.value = false
    }

    return { 
      logo,
      isMobileMenuOpen,
      toggleMobileMenu,
      closeMobileMenu
    }
  }
}
</script>

<style scoped>
.header {
  background: #fff;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  transition: all 0.3s ease;
}

.header.scrolled {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
}

.nav {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 70px;
}

.logo {
  font-size: 28px;
  font-weight: bold;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.logo-img {
  height: 180px;
  display: block;
}

/* 데스크톱 스타일 */
.nav-links {
  display: flex;
  list-style: none;
  gap: 40px;
  margin: 0;
  padding: 0;
}

.nav-links a {
  text-decoration: none;
  color: #333;
  font-weight: 500;
  transition: color 0.3s ease;
  position: relative;
}

.nav-links a:hover {
  color: #667eea;
}

.nav-links a::after {
  content: '';
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 0;
  height: 2px;
  background: #667eea;
  transition: width 0.3s ease;
}

.nav-links a:hover::after {
  width: 100%;
}

/* 햄버거 메뉴 - 기본적으로 숨김 */
.mobile-menu {
  display: none;
  flex-direction: column;
  cursor: pointer;
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
  padding: 10px;
  margin: -10px;
  z-index: 1001;
}

.mobile-menu span {
  width: 25px;
  height: 3px;
  background: #333;
  margin: 3px 0;
  transition: 0.3s;
}

/* 모바일 스타일 */
@media (max-width: 768px) {
  /* 햄버거 메뉴 버튼 보이기 */
  .mobile-menu {
    display: flex;
  }

  /* 네비게이션 메뉴를 사이드바로 변경 */
  .nav-links {
    position: fixed;
    top: 70px;
    right: -100%;
    flex-direction: column;
    background: white;
    width: 250px;
    height: calc(100vh - 70px);
    padding: 20px;
    box-shadow: -2px 0 10px rgba(0, 0, 0, 0.1);
    transition: right 0.3s ease;
    z-index: 999;
    gap: 0; /* 모바일에서는 gap 제거 */
  }

  /* 모바일 메뉴가 열렸을 때 */
  .nav-links.mobile-open {
    right: 0 !important;
  }

  .nav-links li {
    margin: 15px 0;
  }

  .nav-links a {
    font-size: 1.1rem;
    padding: 10px 0;
    display: block;
  }

  /* 햄버거 메뉴 X 모양 변환 */
  .mobile-menu.active span:nth-child(1) {
    transform: rotate(45deg) translate(6px, 6px);
  }

  .mobile-menu.active span:nth-child(2) {
    opacity: 0;
  }

  .mobile-menu.active span:nth-child(3) {
    transform: rotate(-45deg) translate(6px, -6px);
  }
}

/* 오버레이 추가 (선택사항) */
@media (max-width: 768px) {
  .mobile-menu-overlay {
    position: fixed;
    top: 70px;
    left: 0;
    width: 100%;
    height: calc(100vh - 70px);
    background: rgba(0, 0, 0, 0.5);
    z-index: 998;
    opacity: 0;
    visibility: hidden;
    transition: all 0.3s ease;
  }

  .mobile-menu-overlay.active {
    opacity: 1;
    visibility: visible;
  }
}
</style>