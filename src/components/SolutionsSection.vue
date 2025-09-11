<template>
  <section class="section solutions-section" id="solutions">
    <div class="container">
      <h2 class="section-title">Our Solutions</h2>
      <p class="section-subtitle">혁신적인 기술을 바탕으로 다양한 솔루션을 제공합니다</p>
      
      <!-- Solution Navigation -->
      <div class="solution-nav">
        <div class="solution-nav-item">
          <button 
            class="solution-nav-btn" 
            :class="{ active: activeSolution === 'cloudwai' }" 
            @click="changeSolution('cloudwai')"
          >
            CloudWai
          </button>
          <div class="solution-tooltip">
            <div class="tooltip-content">
              <h4>☁️ CloudWai</h4>
              <p>클라우드 인프라 자동구성 및 통합관리 솔루션</p>
              <span class="tooltip-detail">프로비저닝-빌링-모니터링, One-Stop 관리</span>
            </div>
            <div class="tooltip-arrow"></div>
          </div>
        </div>

        <div class="solution-nav-item">
          <button 
            class="solution-nav-btn" 
            :class="{ active: activeSolution === 'kubesync' }" 
            @click="changeSolution('kubesync')"
          >
            KubeSync
          </button>
          <div class="solution-tooltip">
            <div class="tooltip-content">
              <h4>⚙️ KubeSync</h4>
              <p>MSA와 컨테이너 환경을 위한 통합 관리 솔루션</p>
              <span class="tooltip-detail">컨테이너부터 CI/CD·모니터링까지 완전 자동화된 DevOps 시스템</span>
            </div>
            <div class="tooltip-arrow"></div>
          </div>
        </div>

        <div class="solution-nav-item">
          <button 
            class="solution-nav-btn" 
            :class="{ active: activeSolution === 'dapq' }" 
            @click="changeSolution('dapq')"
          >
            DapQ & DataQ
          </button>
          <div class="solution-tooltip">
            <div class="tooltip-content">
              <h4>🤖 DapQ & DataQ</h4>
              <p>생성형 AI 기반 데이터 분석 및 질의응답 솔루션</p>
              <span class="tooltip-detail">AI 채팅 서비스와 자연어-SQL 변환 플랫폼</span>
            </div>
            <div class="tooltip-arrow"></div>
          </div>
        </div>

        <div class="solution-nav-item">
          <button 
            class="solution-nav-btn" 
            :class="{ active: activeSolution === 'neoflow' }" 
            @click="changeSolution('neoflow')"
          >
            NeoFlow
          </button>
          <div class="solution-tooltip">
            <div class="tooltip-content">
              <h4>📊 NeoFlow</h4>
              <p>통합 데이터 플랫폼 구축 솔루션</p>
              <span class="tooltip-detail">데이터 수집, 가공, 적재까지의 전과정 자동화</span>
            </div>
            <div class="tooltip-arrow"></div>
          </div>
        </div>
      </div>

      <!-- Solution Details -->
      <CloudWai v-show="activeSolution === 'cloudwai'" />
      <KubeSync v-show="activeSolution === 'kubesync'" />
      <DapQDataQ v-show="activeSolution === 'dapq'" />
      <NeoFlow v-show="activeSolution === 'neoflow'" />
    </div>
  </section>
</template>

<script>
import CloudWai from './solutions/CloudWai.vue'
import KubeSync from './solutions/KubeSync.vue'
import DapQDataQ from './solutions/DapQDataQ.vue'
import NeoFlow from './solutions/NeoFlow.vue'

export default {
  name: 'SolutionsSection',
  components: {
    CloudWai,
    KubeSync,
    DapQDataQ,
    NeoFlow
  },
  props: {
    activeSolution: {
      type: String,
      default: 'cloudwai'
    }
  },
  emits: ['change-solution'],
  methods: {
    changeSolution(solutionType) {
      this.$emit('change-solution', solutionType)
    }
  }
}
</script>

<style scoped>
.section {
  padding: 100px 0;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.section-title {
  text-align: center;
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 20px;
  color: #333;
}

.section-subtitle {
  text-align: center;
  font-size: 1.2rem;
  color: #666;
  margin-bottom: 60px;
  word-break: keep-all;
}

.solutions-section {
  background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
}

.solution-nav {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 50px;
  flex-wrap: wrap;
}

.solution-nav-item {
  position: relative;
  display: inline-block;
}

.solution-nav-btn {
  padding: 12px 30px;
  border: 2px solid #667eea;
  background: white;
  color: #667eea;
  border-radius: 25px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 1rem;
  position: relative;
  z-index: 2;
}

.solution-nav-btn:hover,
.solution-nav-btn.active {
  background: #667eea;
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
}

/* 기본적으로 tooltip은 숨김 */
.solution-tooltip {
  display: none;
}

.tooltip-content {
  padding: 16px 20px;
}

.tooltip-content h4 {
  margin: 0 0 8px 0;
  color: #667eea;
  font-size: 1.1rem;
  font-weight: 700;
}

.tooltip-content p {
  margin: 0 0 8px 0;
  color: #333;
  font-size: 0.95rem;
  line-height: 1.4;
  font-weight: 500;
}

.tooltip-detail {
  color: #666;
  font-size: 0.85rem;
  line-height: 1.3;
  font-style: italic;
}

.tooltip-arrow {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  width: 0;
  height: 0;
  border-left: 8px solid transparent;
  border-right: 8px solid transparent;
  border-top: 8px solid white;
}

.tooltip-arrow::before {
  content: '';
  position: absolute;
  top: -9px;
  left: -8px;
  width: 0;
  height: 0;
  border-left: 8px solid transparent;
  border-right: 8px solid transparent;
  border-top: 8px solid #e9ecef;
}

/* 활성화된 버튼의 tooltip 스타일 조정 */
.solution-nav-btn.active + .solution-tooltip .tooltip-content h4 {
  color: #5a6fd8;
}

/* Tooltip은 데스크톱에서만 표시 */
@media (min-width: 1025px) {
  .solution-tooltip {
    display: block;
    position: absolute;
    bottom: 120%;
    left: 50%;
    transform: translateX(-50%);
    background: white;
    border-radius: 12px;
    padding: 0;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
    opacity: 0;
    visibility: hidden;
    transition: all 0.3s ease;
    z-index: 1000;
    border: 1px solid #e9ecef;
    /*min-width: 280px;*/
    min-width: 350px;
    max-width: 320px;
  }

  .solution-nav-item:hover .solution-tooltip {
    opacity: 1;
    visibility: visible;
    transform: translateX(-50%) translateY(-5px);
  }

  /* 애니메이션 지연 효과 */
  .solution-nav-item:nth-child(1) .solution-tooltip {
    transition-delay: 0s;
  }

  .solution-nav-item:nth-child(2) .solution-tooltip {
    transition-delay: 0.1s;
  }

  .solution-nav-item:nth-child(3) .solution-tooltip {
    transition-delay: 0.2s;
  }

  .solution-nav-item:nth-child(4) .solution-tooltip {
    transition-delay: 0.3s;
  }
}

/* 태블릿 대응 */
@media (max-width: 1024px) and (min-width: 769px) {
  .solution-nav {
    gap: 18px;
  }
  
  .solution-nav-btn {
    padding: 12px 25px;
  }
}

@media (max-width: 768px) {
  .solution-nav {
    flex-direction: column;
    align-items: center;
    gap: 15px;
  }

  .solution-nav-btn {
    width: 280px; /* 일관된 너비 설정 */
    max-width: 90vw; /* 작은 화면에서 반응형 */
  }
}
</style>