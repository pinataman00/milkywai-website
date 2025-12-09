<template>
  <section class="hero" id="home">
    <!-- 배경 선택 컨트롤 (개발용 - 실제 배포시 제거 가능) -->
    <div class="background-selector" v-if="!showSelector">
      <button @click="$emit('background-changed', 'milkyway')" :class="{ active: backgroundType === 'milkyway' }"
        class="bg-btn">
        🌌 은하수 테크
      </button>
      <button @click="$emit('background-changed', 'cyber')" :class="{ active: backgroundType === 'cyber' }"
        class="bg-btn">
        🔮 사이버 그리드
      </button>
      <button @click="$emit('background-changed', 'minimal')" :class="{ active: backgroundType === 'minimal' }"
        class="bg-btn">
        🐭 미니멀 그리드
      </button>
      <button @click="$emit('background-changed', 'holographic')" :class="{ active: backgroundType === 'holographic' }"
        class="bg-btn">
        👾 홀로그래픽
      </button>
      <!-- 다른 버튼들... -->
    </div>

    <!-- 동적 배경 컴포넌트 렌더링 -->
    <div class="hero-background">
      <component :is="currentBackgroundComponent" />
    </div>

    <!-- 히어로 컨텐츠 -->
    <div class="hero-content">
      <!-- <h1>클라우드·데이터·AI로 여는 비즈니스 혁신</h1> -->
      <h1>데이터·AI로 여는 비즈니스 혁신</h1>
      <p>혁신적인 기술과 전문성을 바탕으로 고객의 경쟁력을 높이는 최적의 솔루션을 제공합니다</p>
      <div class="services-preview">
        <!-- CloudWai 주석처리 (임시 배포용) -->
        <!-- <div class="service-tag" @click="goToSolution('cloudwai')">Cloud Service</div> -->
        <!-- NeoFlow 주석처리 (임시 배포용) -->
        <!-- <div class="service-tag" @click="goToSolution('neoflow')">Data Platform</div> -->
        <div class="service-tag" @click="goToSolution('verora')">AI Chatbot</div>
        <div class="service-tag" @click="goToSolution('dataq')">AI Data Analytics</div>
        <div class="service-tag" @click="goToSolution('dovora')">AI Document</div>
        <div class="service-tag" @click="goToSolution('kubesync')">DevOps Service</div>
      </div>
    </div>
  </section>
</template>

<script>
import { backgroundComponents } from '../components/designs'

export default {
  name: 'HeroSection',
  props: {
    backgroundType: {
      type: String,
      default: 'cyber',
      validator: (value) => ['milkyway', 'minimal', 'cyber', 'fluid', 'neural', 'holographic'].includes(value)
    },
    showSelector: {
      type: Boolean,
      default: false // 개발 모드에서만 true로 설정
    },
  },
  computed: {
    currentBackgroundComponent() {
      return backgroundComponents[this.backgroundType] || backgroundComponents.milkyway
    }
  },
  methods: {
    goToSolution(solutionType) {
      this.$emit('go-to-solution', solutionType)
    }
  }
}
</script>

<style scoped>
.hero {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  /* Header와 겹치지 않도록 상단 여백 확보 */
  padding-top: 70px;
}

.hero-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.hero-background :deep(.background-container) {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.hero-background :deep(.background-svg) {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.background-selector {
  position: absolute;
  top: 80px;
  right: 20px;
  /* z-index: 10; */
  /* TODO 250908) z-index 수정 */
  z-index: 1000;
  display: flex;
  gap: 10px;
  background: rgba(0, 0, 0, 0.5);
  padding: 10px;
  border-radius: 10px;
  backdrop-filter: blur(10px);
}

.bg-btn {
  padding: 8px 16px;
  border: 1px solid rgba(255, 255, 255, 0.3);
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 0.9rem;
  font-weight: 500;
}

.bg-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
}

.bg-btn.active {
  background: rgba(255, 255, 255, 0.3);
  border-color: rgba(255, 255, 255, 0.6);
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.3);
}

.hero-content {
  position: relative;
  z-index: 1;
  color: #fff;
  max-width: min(1100px, 96vw);
  padding: 0 20px;
  text-align: center;
  display: flex;
  flex-direction: column;
  /* h1, p 세로로 쌓기 */
  align-items: center;
  /* 가로축 중앙 */
  justify-content: center;
  /* 세로축 중앙 */
}

.hero h1 {
  font-size: 3.5rem;
  font-weight: 700;
  margin-bottom: 20px;
  opacity: 0;
  transform: translateY(30px);
  animation: fadeInUp 1s ease forwards;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);

  line-height: 1.2; /* TODO 250908 추가함 */
}

.hero p {
  font-size: 1.3rem;
  margin-bottom: 40px;
  opacity: 0;
  transform: translateY(30px);
  animation: fadeInUp 1s ease 0.2s forwards;
  text-shadow: 0 1px 5px rgba(0, 0, 0, 0.3);
  /* TODO 아래 내용 추가함 */
  line-height: 1.6;
}

.services-preview {
  display: flex;
  gap: 20px;
  justify-content: center;
  flex-wrap: wrap;
  opacity: 0;
  transform: translateY(30px);
  animation: fadeInUp 1s ease 0.4s forwards;
}

.service-tag {
  background: rgba(255, 255, 255, 0.15);
  padding: 12px 24px;
  border-radius: 30px;
  font-weight: 500;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  transition: all 0.3s ease;
  cursor: pointer;
  user-select: none;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  /* TODO 250908) */
  min-height: 48px;
}

.service-tag:hover {
  transform: translateY(-2px);
  background: rgba(255, 255, 255, 0.25);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
  border-color: rgba(255, 255, 255, 0.4);
}

.service-tag:active {
  transform: translateY(0px);
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

/* 태블릿 */
@media (max-width: 1024px) {
  .hero h1 {
    font-size: 3rem;
  }
  
  .hero p {
    font-size: 1.2rem;
  }
}

/* 작은 모바일 */
@media (max-width: 480px) {
  .hero {
    /* 작은 화면에서도 충분한 여백 확보 */
    padding-top: 80px;
  }

  .hero h1 {
    font-size: 1.8rem;
  }

  .hero p {
    font-size: 0.9rem;
  }
}

/* 모바일 대응 */
@media (max-width: 768px) {

  .hero {
    /* 모바일에서 Header와 더 많은 여백 확보 */
    padding-top: 90px;
  }

  .hero-content {
    padding: 0 30px; /* 좌우 여백 증가 */
  }


  .hero h1 {
    /* font-size: 2.5rem; */
    font-size: 2.2rem;
    line-height: 1.3;
    margin-bottom: 15px;
  }

  .hero p {
    /* font-size: 1.1rem; */
    font-size: 1rem;
    line-height: 1.5;
    margin-bottom: 30px;
  }

  .services-preview {
    flex-direction: column;
    align-items: center;
    gap: 15px;
  }

  .service-tag {
    width: 90%;
    max-width: 400px;
    text-align: center;
  }

  .background-selector {
    flex-direction: column;
    gap: 8px;
    top: 10px;
    right: 10px;
  }

  .bg-btn {
    padding: 6px 12px;
    font-size: 0.8rem;
    /* TODO 250908) 테스트 */
    touch-action: manipulation;
    /* 추가 */
    -webkit-tap-highlight-color: transparent;
    /* 추가 */
  }
}

/* 반응형 SVG 최적화 */
/* @media (max-width: 1024px) {
  .hero-svg {
    transform: scale(1.1);
  }
}

@media (max-width: 480px) {
  .hero-svg {
    transform: scale(1.2);
  }
} */
</style>