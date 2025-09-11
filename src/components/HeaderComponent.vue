<template>
  <header class="header" :class="{ scrolled: isScrolled }">
    <nav class="nav">
      <a class="logo" href="#home">
        <img :src="logo" alt="milkywai logo" class="logo-img" />
      </a>

      <!-- 데스크톱/모바일 메뉴 -->
      <ul class="nav-links" :class="{ 'mobile-open': isMobileMenuOpen }">
        <li><a href="#home" @click="closeMobileMenu">Home</a></li>
        <li><a href="#services" @click="closeMobileMenu">Services</a></li>
        <li class="solutions-dropdown" @mouseenter="showDropdown" @mouseleave="hideDropdown">
          <a href="#solutions" @click="handleSolutionsClick" class="solutions-main-link">
            Solutions
            <!-- PC와 모바일 상태 모두 반영 -->
            <svg class="dropdown-arrow" :class="{ open: isDropdownOpen || isMobileSolutionsOpen }" width="12" height="8" viewBox="0 0 12 8" fill="none">
              <path d="M1 1.5L6 6.5L11 1.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" />
            </svg>
          </a>

          <!-- 데스크톱 드롭다운 메뉴 (먼저 배치) -->
          <div class="dropdown-menu" :class="{ visible: isDropdownOpen }">
            <div class="dropdown-content">
              <div class="dropdown-header">
                <h3>Our Solutions</h3>
                <p>혁신적인 기술을 바탕으로 다양한 솔루션을 제공합니다</p>
              </div>
              <div class="solutions-grid">
                <a v-for="solution in solutions" :key="solution.id" href="#solutions" class="solution-item"
                  @click="handleSolutionClick(solution.id)">
                  <div class="solution-icon">{{ solution.icon }}</div>
                  <div class="solution-info">
                    <h4>{{ solution.name }}</h4>
                    <p>{{ solution.description }}</p>
                  </div>
                </a>
              </div>
            </div>
          </div>

          <!-- 모바일 서브메뉴 (나중에 배치) -->
          <div class="mobile-submenu" :class="{ visible: isMobileSolutionsOpen }">
            <a v-for="solution in solutions" :key="solution.id" href="#solutions" class="mobile-solution-item"
              @click="handleSolutionClick(solution.id)">
              <span class="mobile-solution-icon">{{ solution.icon }}</span>
              <div class="mobile-solution-info">
                <span class="mobile-solution-name">{{ solution.name }}</span>
                <span class="mobile-solution-desc">{{ solution.description }}</span>
              </div>
            </a>
          </div>
        </li>
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

    <!-- 모바일 메뉴 오버레이 -->
    <div v-if="isMobileMenuOpen" class="mobile-menu-overlay" @click="closeMobileMenu"></div>
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
  emits: ['solution-selected'],
  setup(props, { emit }) {
    const isMobileSolutionsOpen = ref(false)
    const isMobileMenuOpen = ref(false)
    const isDropdownOpen = ref(false)
    let dropdownTimeout = null

    // 솔루션 데이터 (SolutionSection과 동일하게 맞춤)
    const solutions = ref([
      {
        id: 'cloudwai',
        name: 'CloudWai',
        description: '클라우드 인프라 자동구성 및 통합관리',
        icon: '☁️'
      },
      {
        id: 'kubesync',
        name: 'KubeSync',
        description: 'MSA와 컨테이너 환경 통합 관리',
        icon: '⚙️'
      },
      {
        id: 'dapq',
        name: 'DapQ & DataQ',
        description: '생성형 AI 기반 데이터 분석 및 질의응답',
        icon: '🤖'
      },
      {
        id: 'neoflow',
        name: 'NeoFlow',
        description: '통합 데이터 플랫폼 구축',
        icon: '📊'
      }
    ])

    const toggleMobileMenu = () => {
      isMobileMenuOpen.value = !isMobileMenuOpen.value
      if (isMobileMenuOpen.value) {
        isDropdownOpen.value = false
      } else {
        isMobileSolutionsOpen.value = false
      }
    }

    const closeMobileMenu = () => {
      isMobileMenuOpen.value = false
      isMobileSolutionsOpen.value = false
    }

    const showDropdown = () => {
      // 모바일에서는 드롭다운 표시하지 않음
      if (window.innerWidth <= 768) return

      if (dropdownTimeout) {
        clearTimeout(dropdownTimeout)
        dropdownTimeout = null
      }
      isDropdownOpen.value = true
    }

    const hideDropdown = () => {
      dropdownTimeout = setTimeout(() => {
        isDropdownOpen.value = false
      }, 150)
    }

    const handleSolutionsClick = (event) => {
      event.preventDefault()

      // 모바일에서는 서브메뉴 토글
      if (window.innerWidth <= 768 && isMobileMenuOpen.value) {
        isMobileSolutionsOpen.value = !isMobileSolutionsOpen.value
        return
      }

      // 데스크톱에서는 Solutions 섹션으로 이동
      scrollToSection('#solutions')
      closeMobileMenu()
    }

    const handleSolutionClick = (solutionId) => {
      // 부모 컴포넌트에 선택된 솔루션 정보 전달
      emit('solution-selected', solutionId)

      // Solutions 섹션으로 이동
      scrollToSection('#solutions')

      // 모든 메뉴 닫기
      isDropdownOpen.value = false
      closeMobileMenu()
    }

    const scrollToSection = (selector) => {
      const section = document.querySelector(selector)
      if (section) {
        const offsetTop = section.offsetTop - 70
        window.scrollTo({
          top: offsetTop,
          behavior: 'smooth'
        })
      }
    }

    return {
      logo,
      isMobileMenuOpen,
      isDropdownOpen,
      isMobileSolutionsOpen,
      solutions,
      toggleMobileMenu,
      closeMobileMenu,
      showDropdown,
      hideDropdown,
      handleSolutionsClick,
      handleSolutionClick,
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
  text-decoration: none;
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
  display: flex;
  align-items: center;
  gap: 5px;
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

/* 드롭다운 관련 스타일 */
.solutions-dropdown {
  position: relative;
}

.dropdown-arrow {
  transition: transform 0.3s ease;
  margin-left: 4px;
  display: none; /* PC에서는 화살표 숨김 */
}

.dropdown-arrow.open {
  transform: rotate(180deg);
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background: white;
  border-radius: 12px;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.15);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  min-width: 600px;
  margin-top: 10px;
  border: 1px solid #f0f0f0;
  z-index: 1001;
}

.dropdown-menu.visible {
  opacity: 1;
  visibility: visible;
}

.dropdown-content {
  padding: 30px;
}

.dropdown-header {
  text-align: center;
  margin-bottom: 25px;
  padding-bottom: 20px;
  border-bottom: 1px solid #f0f0f0;
}

.dropdown-header h3 {
  font-size: 1.5rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 8px;
}

.dropdown-header p {
  color: #666;
  font-size: 0.9rem;
}

.solutions-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.solution-item {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 20px;
  border-radius: 10px;
  transition: all 0.3s ease;
  text-decoration: none;
  color: #333;
  border: 1px solid transparent;
}

.solution-item:hover {
  background: #f8f9ff;
  border-color: #667eea;
  transform: translateY(-2px);
  box-shadow: 0 5px 20px rgba(102, 126, 234, 0.1);
}

.solution-item:hover::after {
  display: none;
}

.solution-icon {
  font-size: 2rem;
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 12px;
  color: white;
  flex-shrink: 0;
}

.solution-info h4 {
  font-size: 1.1rem;
  font-weight: 600;
  margin-bottom: 5px;
  color: #333;
}

.solution-info p {
  font-size: 0.85rem;
  color: #666;
  line-height: 1.4;
  margin: 0;
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

/* 모바일 메뉴 오버레이 */
.mobile-menu-overlay {
  position: fixed;
  top: 70px;
  left: 0;
  width: 100%;
  height: calc(100vh - 70px);
  background: rgba(0, 0, 0, 0.5);
  z-index: 998;
  display: none;
}

/* 모바일 서브메뉴 (데스크톱에서는 숨김) */
.mobile-submenu {
  display: none;
}

/* 모바일 스타일 */
@media (max-width: 768px) {
  /* 햄버거 메뉴 버튼 보이기 */
  .mobile-menu {
    display: flex;
  }

  /* 모바일 메뉴 오버레이 보이기 */
  .mobile-menu-overlay {
    display: block;
  }

  /* 네비게이션 메뉴를 사이드바로 변경 */
  .nav-links {
    position: fixed;
    top: 70px;
    right: -100%;
    flex-direction: column;
    background: white;
    width: 280px;
    height: calc(100vh - 70px);
    padding: 20px;
    box-shadow: -2px 0 10px rgba(0, 0, 0, 0.1);
    transition: right 0.3s ease;
    z-index: 999;
    gap: 0;
    overflow-y: auto;
  }

  /* 모바일 메뉴가 열렸을 때 */
  .nav-links.mobile-open {
    right: 0 !important;
  }

  /* 모바일 메뉴 아이템 스타일 - border 제거하고 깔끔하게 */
  .nav-links li {
    margin: 0;
  }

  /* Solutions 메뉴 아이템만 특별 처리 */
  .solutions-dropdown {
    position: relative;
    border-bottom: 1px solid #f0f0f0;
  }

  .solutions-main-link {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 1.1rem;
    padding: 15px 0;
    color: #333;
    text-decoration: none;
    font-weight: 500;
    width: 100%;
  }

  .solutions-main-link:hover::after {
    display: none;
  }

  /* 모바일에서 Solutions 화살표 보이기 */
  .solutions-main-link .dropdown-arrow {
    display: block;
    margin-left: auto;
    color: #667eea;
  }

  /* 모바일 서브메뉴 표시 */
  .mobile-submenu {
    display: block;
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.3s ease;
    background: #f8f9ff;
    margin: 0 -20px;
    border-top: 1px solid #e0e6ff;
  }

  .mobile-submenu.visible {
    max-height: 400px;
  }

  .mobile-solution-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 15px 20px;
    text-decoration: none;
    color: #333;
    transition: background-color 0.3s ease;
    border-bottom: 1px solid #e0e6ff;
  }

  .mobile-solution-item:last-child {
    border-bottom: none;
  }

  .mobile-solution-item:hover {
    background: #eef2ff;
  }

  .mobile-solution-icon {
    font-size: 1.5rem;
    width: 35px;
    height: 35px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    border-radius: 8px;
    color: white;
    flex-shrink: 0;
  }

  .mobile-solution-info {
    display: flex;
    flex-direction: column;
    gap: 2px;
  }

  .mobile-solution-name {
    font-weight: 600;
    font-size: 1rem;
    color: #333;
  }

  .mobile-solution-desc {
    font-size: 0.8rem;
    color: #666;
    line-height: 1.3;
  }

  /* 일반 메뉴 아이템 (Solutions 제외) */
  .nav-links a:not(.solutions-main-link):not(.mobile-solution-item) {
    font-size: 1.1rem;
    padding: 15px 0;
    display: block;
    color: #333;
    text-decoration: none;
    font-weight: 500;
    border-bottom: 1px solid #f0f0f0;
  }

  /* 마지막 메뉴 아이템의 border 제거 */
  .nav-links li:last-child a {
    border-bottom: none;
  }

  /* 데스크톱 드롭다운 숨김 */
  .dropdown-menu {
    display: none;
  }

  /* 모바일에서는 Solutions 화살표만 보이기 */
  .nav-links a:not(.solutions-main-link) .dropdown-arrow {
    display: none;
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
</style>