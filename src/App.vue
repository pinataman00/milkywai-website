<template>
  <div id="app">
    <!-- Header -->
    <HeaderComponent :isScrolled="isScrolled" />

    <!-- Hero Section -->
    <HeroSection @go-to-solution="goToSolution" />

    <!-- Services Section -->
    <ServicesSection />

    <!-- Solutions Section -->
    <SolutionsSection :activeSolution="activeSolution" @change-solution="changeSolution" />

    <!-- Company Info Section -->
    <CompanyInfoSection />

    <!-- Contact Section -->
    <ContactSection />
  </div>
</template>

<script>
import HeaderComponent from './components/HeaderComponent.vue'
import HeroSection from './components/HeroSection.vue'
import ServicesSection from './components/ServicesSection.vue'
import SolutionsSection from './components/SolutionsSection.vue'
import CompanyInfoSection from './components/CompanyInfoSection.vue'
import ContactSection from './components/ContactSection.vue'

export default {
  name: 'App',
  components: {
    HeaderComponent,
    HeroSection,
    ServicesSection,
    SolutionsSection,
    CompanyInfoSection,
    ContactSection
  },
  data() {
    return {
      isScrolled: false,
      activeSolution: 'cloudwai'
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
    }
  }
}
</script>

<style>
@import './styles/main.css';
</style>