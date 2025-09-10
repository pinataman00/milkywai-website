<template>
  <section class="services-section" id="services">
    <div class="container">
      <!-- Header -->
      <div class="section-header">
        <!-- <h2 class="section-title">클라우드부터 AI까지, 디지털 혁신의 완성</h2> -->
        <h2 class="section-title">비용 절감부터 생산성 향상까지 <br/> 눈에 보이는 변화를 만듭니다</h2>
        <p class="section-subtitle">전문 기술팀의 검증된 역량으로
          클라우드 구축부터 AI 도입까지 단계별 맞춤 서비스를 제공합니다</p>
      </div>

      <!-- Navigation -->
      <nav class="service-nav">
        <div v-for="nav in navigationItems" :key="nav.category" class="nav-item"
          :class="{ active: activeCategory === nav.category }" @click="setActiveCategory(nav.category)">
          {{ nav.name }}
        </div>
      </nav>

      <!-- Content -->
      <div class="services-content">
        <!-- Featured Services (전체 서비스) -->
        <div class="service-category" :class="{ active: activeCategory === 'all' }">
          <!-- Featured Services Header -->
          <div class="featured-header">
            <h3 class="featured-title">밀키웨이의 서비스를 소개합니다</h3>
            <p class="featured-subtitle">
              디지털 혁신에 필요한 모든 것
            </p>
          </div>

          <!-- Featured Services Grid (데스크톱/태블릿) -->
          <div class="featured-services-grid">
            <div v-for="(service, index) in featuredServices" :key="service.category" class="featured-service-card"
              :class="{ highlighted: highlightedService === service.category }"
              @click="setActiveCategory(service.category)" @mouseenter="highlightServiceFromFeatured(service.category)"
              @mouseleave="clearHighlight">
              <div class="featured-icon">{{ service.icon }}</div>
              <h4 class="featured-service-title">{{ service.title }}</h4>
              <p class="featured-service-desc">{{ service.description }}</p>
              <div class="featured-highlights">
                <div v-for="highlight in service.highlights" :key="highlight" class="featured-highlight">
                  {{ highlight }}
                </div>
              </div>
            </div>
          </div>

          <!-- Mobile Keywords (모바일 전용) -->
          <div class="mobile-services-keywords">
            <button v-for="service in featuredServices" :key="`mobile-${service.category}`" class="mobile-service-btn"
              @click="setActiveCategory(service.category)">
              <span class="mobile-service-icon">{{ service.icon }}</span>
              <div class="mobile-service-title">{{ service.title }}</div>
              <div class="mobile-service-keyword">{{ service.highlights.join(' · ') }}</div>
            </button>
          </div>

          <!-- Stats Section -->
          <div class="stats-section">
            <h3 class="stats-title">밀키웨이와 함께하는 성과</h3>
            <div class="stats-grid">
              <div v-for="stat in statsData" :key="stat.label" class="stat-item">
                <div class="stat-number">{{ stat.number }}</div>
                <div class="stat-label">{{ stat.label }}</div>
                <div class="stat-desc">{{ stat.desc }}</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Individual Service Categories -->
        <div v-for="service in servicesData" :key="service.category" class="service-category"
          :class="{ active: activeCategory === service.category }">
          <div class="service-detail">
            <div class="service-info">
              <div class="service-badge">{{ service.badge }}</div>
              <h3 class="service-name">{{ service.name }}</h3>
              <p class="service-description">{{ service.description }}</p>
              <div class="service-highlights">
                <div v-for="highlight in service.highlights" :key="highlight" class="highlight-item">
                  {{ highlight }}
                </div>
              </div>
              <div class="service-actions">
                <a href="#contact" class="action-btn btn-primary" @click="handleContactClick">
                  {{ service.primaryAction }}
                </a>
                <button class="action-btn btn-secondary" @click="handleSecondaryClick(service)">
                  {{ service.secondaryAction }}
                </button>
              </div>
            </div>
            <div class="service-visual" @mouseenter="handleVisualHover" @mouseleave="handleVisualLeave">
              <div class="visual-icon">{{ service.icon }}</div>
              <h4 class="visual-title">{{ service.visualTitle }}</h4>
              <div class="visual-features">
                <div v-for="feature in service.visualFeatures" :key="feature" class="visual-feature">
                  {{ feature }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, reactive, onMounted, onUnmounted } from 'vue'

// emit 정의 추가 (script setup 최상단에)
const emit = defineEmits(['go-to-solution'])

// Reactive data
const activeCategory = ref('all')
const highlightedService = ref(null)

// 서비스-솔루션 매핑 (컴포넌트 내부에서 정의)
const serviceSolutionMapping = {
  'consulting': null,
  'cloud': 'cloudwai',
  'ai': 'dapq',
  // 'data': 'datawai',
  'data': 'neoflow',
  'devops': 'kubesync'
}

const solutionServiceMapping = {
  'cloudwai': 'cloud',
  'dapq': 'ai',
  'neoflow': 'data',
  'kubesync': 'devops'
}

// 네비게이션 아이템
const navigationItems = reactive([
  { category: 'all', name: '전체 서비스' },
  { category: 'consulting', name: '컨설팅' },
  { category: 'cloud', name: '클라우드' },
  { category: 'ai', name: 'AI' },
  { category: 'data', name: '데이터' },
  { category: 'devops', name: 'DevOps' }
])

// Stats 데이터
const statsData = reactive([
  { number: '25+', label: '전문 인력', desc: '클라우드·AI 전문가' },
  { number: '84%', label: '기술직 비율', desc: '높은 기술 전문성' },
  { number: '5', label: '자체 솔루션', desc: '검증된 플랫폼' },
  { number: '24/7', label: '기술 지원', desc: '상시 지원 체계' }
])

// Featured Services
const featuredServices = reactive([
  {
    category: 'consulting',
    icon: '💡',
    title: '컨설팅',
    description: '클라우드 전략 수립부터 데이터 활용까지 디지털 전환의 시작점',
    highlights: ['전략 수립', '아키텍처 설계', '비용 최적화']
  },
  {
    category: 'cloud',
    icon: '☁️',
    title: '클라우드',
    description: 'CloudWai와 함께, 클라우드 인프라 자동 구성부터 통합 관리까지',
    highlights: ['자동 프로비저닝', '통합 빌링', '실시간 모니터링']
  },
  {
    category: 'ai',
    icon: '🤖',
    title: 'AI',
    description: '생성형 AI 기반의 데이터 분석과 질의응답, 해답은 DapQ와 DataQ입니다',
    highlights: ['AI 채팅', 'SQL 자동변환', 'RAG 검색']
  },
  {
    category: 'data',
    icon: '📊',
    title: '데이터',
    description: 'NeoFlow로 데이터 수집, 가공, 적재까지 통합 데이터 플랫폼을 구축해보세요',
    highlights: ['데이터 통합', 'ETL 자동화', '품질 관리']
  },
  {
    category: 'devops',
    icon: '⚙️',
    title: 'DevOps',
    description: 'KubeSync로 MSA 환경의 CI/CD부터 관측 가능성까지 완전 자동화할 수 있습니다',
    highlights: ['CI/CD 자동화', '컨테이너 관리', '모니터링']
  }
])

// Services 데이터
const servicesData = reactive([
  {
    category: 'consulting',
    badge: 'Consulting',
    name: '클라우드/데이터 컨설팅',
    description: '클라우드 환경 최적화와 데이터 분석을 통해 기업의 비즈니스 모델 변화를 지원하며, 데이터 기반 의사결정을 통해 경쟁력을 높이고 비즈니스 효율성을 촉진합니다',
    highlights: [
      '클라우드 마이그레이션 전략 수립',
      '인프라 아키텍처 최적화 설계',
      '데이터 플랫폼 구축 컨설팅',
      '비용 최적화 및 운영 효율성 개선'
    ],
    primaryAction: '상담 신청',
    secondaryAction: '포트폴리오 보기',
    secondaryLink: '#portfolio',
    icon: '💡',
    visualTitle: '전략적 클라우드 컨설팅',
    visualFeatures: ['아키텍처 설계', '비용 최적화', '보안 강화', '성능 튜닝']
  },
  {
    category: 'cloud',
    badge: 'Infrastructure',
    name: '클라우드 구축 및 운영',
    description: '기업의 요구에 맞춰 최적화된 클라우드 인프라를 설계하고 구축합니다. 서버, 스토리지, 네트워킹 등 전체적인 관리 서비스를 통해 안정적이고 확장 가능한 환경을 제공합니다',
    highlights: [
      '멀티클라우드 환경 구축',
      '자동 프로비저닝 및 스케일링',
      '통합 모니터링 및 알림 시스템',
      '보안 정책 및 접근 제어 관리'
    ],
    primaryAction: '구축 문의',
    secondaryAction: 'CloudWai 보기',
    secondaryLink: '#solutions',
    solutionTarget: 'cloudwai',
    icon: '☁️',
    visualTitle: '완전 관리형 클라우드',
    visualFeatures: ['자동 프로비저닝', '통합 빌링', '실시간 모니터링', '토폴로지 맵']
  },
  {
    category: 'ai',
    badge: 'AI Analytics',
    name: 'AI 구축 및 운영',
    description: '생성형 AI와 RAG 기술을 활용한 지능형 서비스를 구축합니다. 기업 내부 데이터를 활용한 AI 채팅 서비스와 자연어를 SQL로 변환하는 데이터 분석 플랫폼을 제공합니다',
    highlights: [
      '생성형 AI 채팅 서비스 (DapQ)',
      '자연어 SQL 변환 플랫폼 (DataQ)',
      'RAG 기반 검색 시스템',
      'AI 기반 문서 처리 및 분석'
    ],
    primaryAction: 'AI 솔루션 상담',
    secondaryAction: 'DapQ & DataQ 보기',
    secondaryLink: '#solutions',
    solutionTarget: 'dapq',
    icon: '🤖',
    visualTitle: '지능형 AI 플랫폼',
    visualFeatures: ['RAG 기반 검색', '자연어 처리', '실시간 분석', '맞춤형 학습']
  },
  {
    category: 'data',
    badge: 'Data Integration',
    name: '데이터 플랫폼 구축',
    description: '다양한 데이터 소스로부터 수집, 가공, 적재까지 전 과정을 자동화하여 기업의 데이터 통합 관리를 간편하게 만들어주는 종합 데이터 플랫폼을 구축합니다',
    highlights: [
      '다양한 데이터 소스 통합 연결',
      'ETL 프로세스 완전 자동화',
      '데이터 처리 스케줄링 및 모니터링',
      '데이터 품질 검증 및 관리'
    ],
    primaryAction: '데이터 상담',
    secondaryAction: 'NeoFlow 보기',
    secondaryLink: '#solutions',
    solutionTarget: 'neoflow',
    icon: '📊',
    visualTitle: '통합 데이터 플랫폼',
    visualFeatures: ['소스 연계', 'ETL 자동화', '스케줄링', '품질 관리']
  },
  {
    category: 'devops',
    badge: 'DevOps',
    name: 'Solution / DevOps',
    description: 'MSA와 컨테이너 환경에서 소프트웨어 개발, 테스트, 배포 과정을 자동화합니다. CI/CD 파이프라인과 관측 가능성을 통해 개발 속도와 서비스 안정성을 동시에 확보합니다',
    highlights: [
      '컨테이너 오케스트레이션 (Kubernetes)',
      'CI/CD 파이프라인 자동화',
      '관측 가능성 (Observability) 구현',
      'Auto Scaling 및 성능 최적화'
    ],
    primaryAction: 'DevOps 문의',
    secondaryAction: 'KubeSync 보기',
    secondaryLink: '#solutions',
    solutionTarget: 'kubesync',
    icon: '⚙️',
    visualTitle: '완전 자동화 DevOps',
    visualFeatures: ['CI/CD 자동화', '컨테이너 관리', '모니터링', '오토 스케일링']
  }
])

// Solutions Section의 버튼들을 하이라이트하는 함수
const highlightSolutionButton = (solutionType, highlight = true) => {
  // 전역 메서드 사용 (더 안정적)
  if (window.solutionsSection) {
    window.solutionsSection.highlightButton(solutionType, highlight)
  }

  // 기존 DOM 방식은 fallback으로 유지
  const solutionButton = document.querySelector(`[data-solution="${solutionType}"]`)
  if (solutionButton) {
    if (highlight) {
      solutionButton.classList.add('highlighted')
    } else {
      solutionButton.classList.remove('highlighted')
    }
  }
}

// Methods
const setActiveCategory = (category) => {
  activeCategory.value = category
}

const highlightServiceFromFeatured = (serviceCategory) => {
  highlightedService.value = serviceCategory

  const linkedSolution = serviceSolutionMapping[serviceCategory]
  if (linkedSolution) {
    // Solutions Section의 해당 버튼 하이라이트
    highlightSolutionButton(linkedSolution, true)
  }
}

const clearHighlight = () => {
  highlightedService.value = null

  // 모든 솔루션 버튼 하이라이트 해제
  Object.values(serviceSolutionMapping).forEach(solution => {
    if (solution) {
      highlightSolutionButton(solution, false)
    }
  })
}

const handleContactClick = (event) => {
  event.preventDefault()
  const contactSection = document.querySelector('#contact')
  if (contactSection) {
    contactSection.scrollIntoView({
      behavior: 'smooth',
      block: 'start'
    })
  }
}

// handleSecondaryClick 함수 수정
const handleSecondaryClick = (service) => {
  if (service.solutionTarget) {
    // ✅ emit을 사용해서 부모로 이벤트 전달
    emit('go-to-solution', service.solutionTarget)

    // URL 해시 업데이트 및 피드백 효과는 유지
    window.location.hash = `solutions-${service.solutionTarget}`
    showNavigationFeedback(service.solutionTarget)

  } else if (service.secondaryLink && service.secondaryLink.startsWith('#')) {
    // 일반적인 앵커 링크 처리
    const target = document.querySelector(service.secondaryLink)
    if (target) {
      target.scrollIntoView({
        behavior: 'smooth',
        block: 'start'
      })
    }
  }
}

/* const handleSecondaryClick = (service) => { // TODO 원본임
  if (service.solutionTarget) {
    // 솔루션 섹션으로 이동하면서 해당 솔루션 활성화
    changeActiveSolution(service.solutionTarget)
    
    const solutionsSection = document.querySelector('#solutions')
    if (solutionsSection) {
      solutionsSection.scrollIntoView({
        behavior: 'smooth',
        block: 'start'
      })
    }
    
    // URL 해시 업데이트
    window.location.hash = `solutions-${service.solutionTarget}`
    
    // 네비게이션 피드백 효과
    showNavigationFeedback(service.solutionTarget)
    
  } else if (service.secondaryLink && service.secondaryLink.startsWith('#')) {
    // 일반적인 앵커 링크 처리
    const target = document.querySelector(service.secondaryLink)
    if (target) {
      target.scrollIntoView({
        behavior: 'smooth',
        block: 'start'
      })
    }
  }
} */

const handleVisualHover = (event) => {
  event.currentTarget.style.transform = 'translateY(-10px) scale(1.02)'
}

const handleVisualLeave = (event) => {
  event.currentTarget.style.transform = 'translateY(0) scale(1)'
}

const showNavigationFeedback = (solutionType) => {
  const linkedServiceCategory = solutionServiceMapping[solutionType]
  if (linkedServiceCategory) {
    highlightedService.value = linkedServiceCategory
    highlightSolutionButton(solutionType, true)

    // 3초 후 하이라이트 해제
    setTimeout(() => {
      clearHighlight()
    }, 3000)
  }
}

// Solutions Section에서 호버 이벤트 리스닝
const handleSolutionHover = (event) => {
  const solutionType = event.detail.solutionType
  const isHovering = event.detail.isHovering

  if (isHovering) {
    const linkedServiceCategory = solutionServiceMapping[solutionType]
    if (linkedServiceCategory) {
      highlightedService.value = linkedServiceCategory
    }
  } else {
    highlightedService.value = null
  }
}

onMounted(() => {
  // Solutions Section의 버튼들에 data 속성 추가 (한번만 실행)
  setTimeout(() => {
    const cloudButton = document.querySelector('.solution-nav-btn:nth-child(1)')
    const kubeButton = document.querySelector('.solution-nav-btn:nth-child(2)')
    const dapqButton = document.querySelector('.solution-nav-btn:nth-child(3)')
    const dataButton = document.querySelector('.solution-nav-btn:nth-child(4)')

    if (cloudButton) cloudButton.setAttribute('data-solution', 'cloudwai')
    if (kubeButton) kubeButton.setAttribute('data-solution', 'kubesync')
    if (dapqButton) dapqButton.setAttribute('data-solution', 'dapq')
    if (dataButton) dataButton.setAttribute('data-solution', 'neoflow')
  }, 100)

  // Solutions Section에서의 호버 이벤트 리스닝
  window.addEventListener('solutionHover', handleSolutionHover)
})

onUnmounted(() => {
  window.removeEventListener('solutionHover', handleSolutionHover)
})

// 외부에서 호출할 수 있는 메소드들 (전역으로 노출)
window.serviceSection = {
  highlightServiceFromFeatured,
  clearHighlight,
  setActiveCategory
}

// Expose methods for parent component
defineExpose({
  setActiveCategory,
  highlightServiceFromFeatured,
  clearHighlight
})
</script>

<style scoped>
.services-section {
  padding: 120px 0;
  background: #fff;
}

.container {
  max-width: 1600px;
  margin: 0 auto;
  padding: 0 40px;
}

/* Header */
.section-header {
  text-align: center;
  margin-bottom: 80px;
}

.section-title {
  font-size: 3.2rem;
  font-weight: 700;
  margin-bottom: 30px;
  color: #333;
  letter-spacing: -0.02em;
}

.section-subtitle {
  font-size: 1.4rem;
  color: #666;
  line-height: 1.8;
  max-width: 900px;
  margin: 0 auto 50px;
}

/* Navigation Tabs */
.service-nav {
  display: flex;
  justify-content: center;
  margin-bottom: 80px;
  border-bottom: 1px solid #e9ecef;
  flex-wrap: wrap;
  gap: 10px;
}

.nav-item {
  padding: 20px 30px;
  font-size: 1.1rem;
  font-weight: 600;
  color: #666;
  cursor: pointer;
  border-bottom: 3px solid transparent;
  transition: all 0.3s ease;
  position: relative;
  white-space: nowrap;
}

.nav-item:hover {
  color: #667eea;
}

.nav-item.active {
  color: #667eea;
  border-bottom-color: #667eea;
}

/* Content Area */
.services-content {
  min-height: 600px;
}

.service-category {
  display: none;
  animation: fadeInUp 0.6s ease forwards;
}

.service-category.active {
  display: block;
}

/* Featured Services */
.featured-header {
  text-align: center;
  margin-bottom: 60px;
}

.featured-title {
  font-size: 2.2rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 20px;
}

.featured-subtitle {
  font-size: 1.2rem;
  color: #666;
  line-height: 1.7;
}

.featured-services-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 30px;
  margin-bottom: 80px;
}

.featured-service-card {
  background: white;
  border-radius: 20px;
  padding: 35px 25px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  border: 2px solid transparent;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}

.featured-service-card.highlighted {
  transform: translateY(-8px) scale(1.02);
  box-shadow: 0 20px 50px rgba(118, 75, 162, 0.25);
  border-color: #764ba2;
  background: linear-gradient(135deg, #fff 0%, #f8f9ff 100%);
}

.featured-service-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  transform: scaleX(0);
  transition: transform 0.3s ease;
}

.featured-service-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.15);
}

.featured-service-card:hover::before {
  transform: scaleX(1);
}

.featured-icon {
  width: 70px;
  height: 70px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  margin: 0 auto 20px;
  color: white;
}

.featured-service-title {
  font-size: 1.3rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 12px;
  text-align: center;
}

.featured-service-desc {
  color: #666;
  line-height: 1.6;
  margin-bottom: 20px;
  text-align: center;
  font-size: 0.95rem;
  word-break: keep-all;
}

.featured-highlights {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  justify-content: center;
  margin-bottom: 25px;
}

.featured-highlight {
  background: #f8f9fa;
  color: #667eea;
  padding: 5px 12px;
  border-radius: 15px;
  font-size: 0.8rem;
  font-weight: 600;
  border: 1px solid #e9ecef;
}

/* Mobile Services */
.mobile-services-keywords {
  display: none;
}

.mobile-service-btn {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 15px;
  padding: 20px 12px;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
}

.mobile-service-btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 25px rgba(102, 126, 234, 0.4);
}

.mobile-service-icon {
  font-size: 1.8rem;
  margin-bottom: 8px;
  display: block;
}

.mobile-service-title {
  font-size: 1rem;
  font-weight: 700;
  margin-bottom: 6px;
}

.mobile-service-keyword {
  font-size: 0.75rem;
  opacity: 0.9;
  font-weight: 500;
  word-break: keep-all;
}

/* Individual Service Detail */
.service-detail {
  display: grid;
  grid-template-columns: 1fr 1.2fr;
  gap: 80px;
  align-items: center;
  margin-bottom: 100px;
}

.service-detail:nth-child(even) {
  direction: rtl;
}

.service-detail:nth-child(even)>* {
  direction: ltr;
}

.service-info {
  padding: 40px 0;
}

.service-badge {
  display: inline-block;
  background: #667eea;
  color: white;
  padding: 8px 20px;
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: 600;
  margin-bottom: 20px;
}

.service-name {
  font-size: 2.5rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 20px;
  line-height: 1.3;
}

.service-description {
  font-size: 1.2rem;
  color: #666;
  line-height: 1.8;
  margin-bottom: 40px;
  word-break: keep-all;
}

.service-highlights {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-bottom: 40px;
}

.highlight-item {
  display: flex;
  align-items: center;
  font-size: 1rem;
  color: #555;
}

.highlight-item::before {
  content: '→';
  color: #667eea;
  font-weight: bold;
  margin-right: 15px;
  font-size: 1.2rem;
}

.service-actions {
  display: flex;
  gap: 20px;
}

.action-btn {
  padding: 15px 30px;
  border-radius: 8px;
  font-weight: 600;
  text-decoration: none;
  transition: all 0.3s ease;
  border: none;
  cursor: pointer;
  font-size: 1rem;
}

.btn-primary {
  background: #667eea;
  color: white;
}

.btn-primary:hover {
  background: #5a6fd8;
  transform: translateY(-2px);
}

.btn-secondary {
  background: transparent;
  color: #667eea;
  border: 2px solid #667eea;
}

.btn-secondary:hover {
  background: #667eea;
  color: white;
}

/* Visual Elements */
.service-visual {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 20px;
  padding: 50px 40px;
  color: white;
  text-align: center;
  position: relative;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.service-visual::before {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><defs><pattern id="dots" width="10" height="10" patternUnits="userSpaceOnUse"><circle cx="5" cy="5" r="1" fill="rgba(255,255,255,0.1)"/></pattern></defs><rect width="100" height="100" fill="url(%23dots)"/></svg>') repeat;
  animation: float 20s linear infinite;
}

.visual-icon {
  font-size: 4rem;
  margin-bottom: 30px;
  position: relative;
  z-index: 2;
}

.visual-title {
  font-size: 1.8rem;
  font-weight: 700;
  margin-bottom: 20px;
  position: relative;
  z-index: 2;
}

.visual-features {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
  position: relative;
  z-index: 2;
}

.visual-feature {
  background: rgba(255, 255, 255, 0.2);
  padding: 15px;
  border-radius: 10px;
  font-size: 0.9rem;
  font-weight: 600;
  backdrop-filter: blur(10px);
}

/* Stats Section */
.stats-section {
  background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
  padding: 80px 50px;
  border-radius: 20px;
  margin-top: 100px;
}

.stats-title {
  text-align: center;
  font-size: 2.2rem;
  font-weight: 700;
  margin-bottom: 60px;
  color: #333;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 40px;
}

.stat-item {
  text-align: center;
  padding: 30px 20px;
  background: white;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
}

.stat-number {
  font-size: 3rem;
  font-weight: 700;
  color: #667eea;
  margin-bottom: 10px;
}

.stat-label {
  font-size: 1.1rem;
  color: #666;
  font-weight: 600;
}

.stat-desc {
  font-size: 0.9rem;
  color: #999;
  margin-top: 8px;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }

  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes float {

  0%,
  100% {
    transform: translateY(0px);
  }

  50% {
    transform: translateY(-10px);
  }
}

/* Solutions Section 연동을 위한 글로벌 스타일 */
:global(.solution-nav-btn.highlighted) {
  background: #764ba2 !important;
  color: white !important;
  border-color: #764ba2 !important;
  transform: translateY(-3px) !important;
  box-shadow: 0 8px 20px rgba(118, 75, 162, 0.4) !important;
}

/* Responsive Design */
@media (max-width: 1200px) {
  .container {
    max-width: 1200px;
    padding: 0 30px;
  }

  .featured-services-grid {
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
    gap: 25px;
  }

  .service-detail {
    gap: 50px;
  }
}

@media (max-width: 968px) {
  .service-nav {
    margin-bottom: 60px;
    overflow-x: auto;
    padding-bottom: 10px;
    scrollbar-width: none;
    -ms-overflow-style: none;
    justify-content: flex-start;
  }

  .service-nav::-webkit-scrollbar {
    display: none;
  }

  .nav-item {
    padding: 15px 25px;
    font-size: 1rem;
    flex-shrink: 0;
    min-width: fit-content;
  }

  .featured-services-grid {
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
  }

  .featured-service-card {
    padding: 30px 20px;
  }

  .featured-icon {
    width: 60px;
    height: 60px;
    font-size: 1.8rem;
  }

  .service-detail {
    grid-template-columns: 1fr;
    gap: 40px;
  }

  .service-detail:nth-child(even) {
    direction: ltr;
  }

  .section-title {
    font-size: 2.5rem;
  }

  .featured-title {
    font-size: 1.8rem;
  }

  .service-name {
    font-size: 2rem;
  }

  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 30px;
  }

  .visual-features {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 576px) {
  .container {
    padding: 0 20px;
  }

  .services-section {
    padding: 60px 0;
  }

  .section-header {
    margin-bottom: 50px;
  }

  .section-title {
    font-size: 1.8rem;
    line-height: 1.4;
    margin-bottom: 20px;
  }

  .section-subtitle {
    font-size: 1rem;
    line-height: 1.6;
    margin-bottom: 30px;
    word-break: keep-all;
  }

  .service-nav {
    margin-bottom: 40px;
  }

  .nav-item {
    padding: 12px 20px;
    font-size: 0.9rem;
  }

  .featured-services-grid {
    display: none;
  }

  .mobile-services-keywords {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 10px;
    margin-bottom: 40px;
  }

  .featured-header {
    margin-bottom: 30px;
  }

  .featured-title {
    font-size: 1.4rem;
    margin-bottom: 15px;
    line-height: 1.3;
  }

  .featured-subtitle {
    font-size: 0.9rem;
    line-height: 1.5;
  }

  .service-badge {
    font-size: 0.8rem;
    padding: 6px 16px;
  }

  .service-name {
    font-size: 1.6rem;
    line-height: 1.3;
    margin-bottom: 15px;
  }

  .service-description {
    font-size: 1rem;
    line-height: 1.6;
    margin-bottom: 25px;
  }

  .highlight-item {
    font-size: 0.9rem;
    line-height: 1.4;
  }

  .service-actions {
    flex-direction: column;
    gap: 12px;
  }

  .action-btn {
    padding: 14px 24px;
    font-size: 0.95rem;
  }

  .stats-section {
    padding: 40px 20px;
    margin-top: 50px;
  }

  .stats-title {
    font-size: 1.4rem;
    margin-bottom: 30px;
    line-height: 1.3;
  }

  .stats-grid {
    grid-template-columns: 1fr 1fr;
    gap: 15px;
  }

  .stat-item {
    padding: 20px 12px;
  }

  .stat-number {
    font-size: 2rem;
    margin-bottom: 8px;
  }

  .stat-label {
    font-size: 0.9rem;
    margin-bottom: 4px;
  }

  .stat-desc {
    font-size: 0.75rem;
    line-height: 1.3;
  }

  .visual-title {
    font-size: 1.3rem;
    margin-bottom: 15px;
  }

  .visual-features {
    grid-template-columns: 1fr;
    gap: 10px;
  }

  .visual-feature {
    padding: 12px;
    font-size: 0.8rem;
  }
}
</style>