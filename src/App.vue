<template>
  <div id="app">
    <!-- Header -->
    <HeaderComponent :isScrolled="isScrolled" />

    <!-- Hero Section -->
    <!-- <HeroSection @go-to-solution="goToSolution" /> -->
    <!-- <HeroSection :background-type="heroBackgroundType" :show-selector="isDevelopmentMode"
      @go-to-solution="goToSolution" /> -->

    <HeroSection :background-type="heroBackgroundType" :show-selector="isDevelopmentMode"
      @background-changed="heroBackgroundType = $event" @go-to-solution="goToSolution" />

    <!-- Services Section -->
    <!-- <ServicesSection /> -->
    <!-- <ServiceSection /> -->
    <ServiceSection @go-to-solution="goToSolution" />
    <!-- Solutions Section -->
    <SolutionsSection :activeSolution="activeSolution" @change-solution="changeSolution" />

    <!-- Company Info Section -->
    <CompanyInfoSection />

    <!-- Contact Section -->
    <ContactSection />

    <!-- Floating Navigation Section -->
    <FloatingActionButtons @consultation-requested="handleConsultationRequest" />
  </div>
</template>

<script>
import HeaderComponent from './components/HeaderComponent.vue'
import HeroSection from './components/HeroSection.vue'
// import ServicesSection from './components/ServicesSection.vue'
import ServiceSection from './components/ServiceSection.vue'
import SolutionsSection from './components/SolutionsSection.vue'
import CompanyInfoSection from './components/CompanyInfoSection.vue'
import ContactSection from './components/ContactSection.vue'
import FloatingActionButtons from './components/FloatingActionButtons.vue'

export default {
  name: 'App',
  components: {
    HeaderComponent,
    HeroSection,
    // ServicesSection,
    ServiceSection,
    SolutionsSection,
    CompanyInfoSection,
    ContactSection,
    FloatingActionButtons
  },
  data() {
    return {
      isScrolled: false,
      activeSolution: 'cloudwai',
      heroBackgroundType: 'cyber',
      isDevelopmentMode: true // 배포시 false로 변경      
    }
  },
  mounted() {
    window.addEventListener('scroll', this.handleScroll)
    this.addSmoothScrolling()
  },
  beforeUnmount() {
    window.removeEventListener('scroll', this.handleScroll)
  },
  methods: {
    handleScroll() {
      this.isScrolled = window.scrollY > 50
    },
    goToSolution(solutionType) {
      // 해당 솔루션 탭 활성화
      this.activeSolution = solutionType

      // Solutions 섹션으로 스크롤
      const solutionsSection = document.querySelector('#solutions')
      if (solutionsSection) {
        const offsetTop = solutionsSection.offsetTop - 70
        window.scrollTo({
          top: offsetTop,
          behavior: 'smooth'
        })
      }
    },
    changeSolution(solutionType) {
      this.activeSolution = solutionType
    },
    addSmoothScrolling() {
      document.querySelectorAll('a[href^="#"]').forEach(anchor => {
        anchor.addEventListener('click', function (e) {
          e.preventDefault()
          const target = document.querySelector(this.getAttribute('href'))
          if (target) {
            const offsetTop = target.offsetTop - 70
            window.scrollTo({
              top: offsetTop,
              behavior: 'smooth'
            })
          }
        })
      })
    },
    // TODO 상담 요청 처리 로직
    handleConsultationRequest(data) {
      // 상담 요청 처리 로직
      console.log('상담 요청:', data)
      // 예: API 호출, 상담 요청 저장 등
    },
    //TODO 히어로 섹션 수정 중...
    handleGoToSolution(solutionType) {
      // Solutions 섹션으로 스크롤하고 해당 솔루션 탭 활성화
      if (this.$refs.solutionsSection) {
        this.$refs.solutionsSection.activateSolution(solutionType)

        // Solutions 섹션으로 부드럽게 스크롤
        const solutionsElement = this.$refs.solutionsSection.$el
        if (solutionsElement) {
          const offsetTop = solutionsElement.offsetTop - 70
          window.scrollTo({
            top: offsetTop,
            behavior: 'smooth'
          })
        }
      }
    },
    activateSolution(solutionType) {
      this.activeSolution = solutionType
    }
  }
}
</script>

<style>
@import './styles/main.css';

/* .solution-detail {
  animation: fadeInUp 0.6s ease forwards;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
} */
</style>
