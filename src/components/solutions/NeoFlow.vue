<template>
  <div class="solution-detail">
    <div class="solution-hero">
      <div class="solution-hero-content">
        <div class="solution-badge">Data Integration</div>
        <h3>NeoFlow</h3>
        <p class="solution-tagline">통합 데이터 플랫폼 구축 솔루션</p>
        <p class="solution-description">다양한 데이터 소스로부터 수집, 가공, 적재까지 전 과정을 자동화하여 기업의 데이터 통합 관리를 간편하게 만들어주는 종합 솔루션입니다.</p>
      </div>
      <div class="solution-hero-visual">
        <div class="data-diagram">
          <div class="data-icon">📊</div>
          <div class="data-flow" ref="dataFlowContainer">
            <div v-for="(step, index) in pipelineSteps" :key="step.id" class="flow-item" :ref="`flowItem${index}`">
              <div class="data-step" :class="{
                active: step.active,
                completed: step.completed,
                'step-visible': isFlowVisible
              }" @mouseenter="handleStepHover(index, true)" @mouseleave="handleStepHover(index, false)">
                {{ step.name }}
              </div>
              <div v-if="index < pipelineSteps.length - 1" class="data-arrow" :class="{
                active: step.completed,
                'arrow-visible': isFlowVisible
              }">
                <span class="arrow-horizontal">→</span>
                <span class="arrow-vertical">↓</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="solution-features">
      <h4>데이터 플랫폼 기능</h4>
      <div class="features-grid">
        <div class="feature-item" v-for="feature in features" :key="feature.id">
          <div class="feature-icon">{{ feature.icon }}</div>
          <h5>{{ feature.title }}</h5>
          <p>{{ feature.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NeoFlow',
  data() {
    return {
      isFlowVisible: false,
      animationTimer: null,
      pipelineSteps: [
        { id: 1, name: '수집', active: false, completed: false },
        { id: 2, name: '가공', active: false, completed: false },
        { id: 3, name: '적재', active: false, completed: false },
        { id: 4, name: '분석', active: false, completed: false }
      ],
      features: [
        {
          id: 1,
          icon: '🔌',
          title: '다양한 소스 연계',
          description: '데이터베이스, 파일, API... 모든 소스를 유연하게 통합해 하나의 데이터 허브를 완성합니다'
        },
        {
          id: 2,
          icon: '⚙️',
          title: 'ETL 자동화',
          description: '데이터 추출·변환·적재 전 과정을 자동화하여, 압도적인 속도와 효율을 제공합니다'
        },
        {
          id: 3,
          icon: '📅',
          title: '스케줄링 관리',
          description: '지능형 스케줄링과 모니터링으로 끊김 없는 안정적인 데이터 플로우를 구현합니다'
        },
        {
          id: 4,
          icon: '📈',
          title: '데이터 품질 관리',
          description: '정교한 품질 검증과 이상 탐지로, 신뢰할 수 있는 데이터 기반 의사결정을 지원합니다'
        }
      ]
    }
  },
  mounted() {
    // 즉시 플로우 표시
    this.showDataFlow();

    // 1초 후 데모 애니메이션 시작
    setTimeout(() => {
      this.startPipelineDemo();
    }, 1000);
  },
  beforeUnmount() {
    // 컴포넌트 해제 시 타이머 정리
    if (this.animationTimer) {
      clearTimeout(this.animationTimer);
    }
  },
  methods: {
    showDataFlow() {
      // 플로우 요소들을 즉시 표시
      this.isFlowVisible = true;
    },

    handleStepHover(index, isHover) {
      if (isHover) {
        console.log(`Data Step ${index + 1} hovered: ${this.pipelineSteps[index].name}`);
      }
    },

    startPipelineDemo() {
      // 기존 애니메이션 정리
      this.resetPipeline();

      // 단계별 활성화 애니메이션
      this.pipelineSteps.forEach((step, index) => {
        this.animationTimer = setTimeout(() => {
          // 현재 단계 활성화
          step.active = true;

          // 0.8초 후 완료 상태로 변경
          this.animationTimer = setTimeout(() => {
            step.active = false;
            step.completed = true;
          }, 800);
        }, index * 900);
      });

      // 전체 사이클 완료 후 재시작
      const totalDuration = this.pipelineSteps.length * 900 + 800 + 2000;
      this.animationTimer = setTimeout(() => {
        this.startPipelineDemo(); // 무한 반복
      }, totalDuration);
    },

    resetPipeline() {
      // 타이머 정리
      if (this.animationTimer) {
        clearTimeout(this.animationTimer);
      }

      // 파이프라인 상태 초기화
      this.pipelineSteps.forEach(step => {
        step.active = false;
        step.completed = false;
      });
    }
  }
}
</script>

<style scoped>
.solution-detail {
  background: white;
  border-radius: 20px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  opacity: 0;
  animation: fadeInUp 0.8s ease forwards;
}

.solution-hero {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  gap: 60px;
  padding: 60px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.solution-badge {
  display: inline-block;
  padding: 8px 20px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: 600;
  margin-bottom: 20px;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
}

.solution-hero h3 {
  font-size: 3rem;
  font-weight: 700;
  margin-bottom: 15px;
}

.solution-tagline {
  font-size: 1.4rem;
  font-weight: 600;
  margin-bottom: 20px;
  opacity: 0.9;
}

.solution-description {
  font-size: 1.1rem;
  line-height: 1.8;
  opacity: 0.9;
}

.solution-hero-visual {
  display: flex;
  justify-content: center;
  align-items: center;
}

.data-diagram {
  text-align: center;
  position: relative;
  width: 100%;
}

.data-icon {
  font-size: 4rem;
  margin-bottom: 30px;
  opacity: 0.9;
  animation: float 3s ease-in-out infinite;
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

/* ✅ 수정된 데이터 파이프라인 스타일 */
.data-flow {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  margin-top: 20px;
  min-height: 80px;
  /* 최소 높이 보장 */
}

.flow-item {
  display: flex;
  align-items: center;
  gap: 20px;
}

/* ✅ 핵심 수정: 초기 상태에서도 보이도록 설정 */
.data-step {
  background: rgba(255, 255, 255, 0.2);
  padding: 15px 35px;
  border-radius: 25px;
  font-size: 1rem;
  font-weight: 600;
  backdrop-filter: blur(5px);
  border: 2px solid rgba(255, 255, 255, 0.3);
  min-width: 50px;
  text-align: center;
  transition: all 0.3s ease;
  cursor: pointer;
  position: relative;
  white-space: nowrap;
  /* ✅ 초기에도 보이도록 변경 */
  opacity: 0.7;
  transform: translateY(0);
}

/* ✅ 보임 상태 클래스 추가 */
.data-step.step-visible {
  opacity: 1;
  animation: stepSlideIn 0.6s ease forwards;
}

.data-step:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: translateY(-3px) scale(1.05);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.data-arrow {
  font-size: 1.5rem;
  font-weight: bold;
  color: rgba(255, 255, 255, 0.6);
  transition: all 0.3s ease;
  /* ✅ 초기에도 보이도록 변경 */
  opacity: 0.7;
  transform: scale(1);
}

/* ✅ 보임 상태 클래스 추가 */
.data-arrow.arrow-visible {
  opacity: 1;
  animation: arrowSlideIn 0.4s ease forwards;
}

.arrow-horizontal {
  display: inline;
}

.arrow-vertical {
  display: none;
}

.data-arrow.active {
  color: rgba(76, 175, 80, 0.9);
  transform: scale(1.1);
  text-shadow: 0 0 10px rgba(76, 175, 80, 0.5);
}

/* ✅ 새로운 애니메이션 */
@keyframes stepSlideIn {
  0% {
    opacity: 0.7;
    transform: translateY(10px);
  }

  100% {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes arrowSlideIn {
  0% {
    opacity: 0.7;
    transform: scale(0.8);
  }

  100% {
    opacity: 1;
    transform: scale(1);
  }
}

/* 진행 상태 표시 */
.data-step.active {
  background: rgba(255, 255, 255, 0.4);
  border-color: rgba(255, 255, 255, 0.7);
  transform: translateY(-3px) scale(1.08);
  box-shadow: 0 12px 35px rgba(0, 0, 0, 0.25);
  animation: pulse 1s ease-in-out infinite;
}

.data-step.completed {
  background: rgba(76, 175, 80, 0.3);
  border-color: rgba(76, 175, 80, 0.7);
}

.data-step.completed::after {
  content: '✓';
  position: absolute;
  top: -8px;
  right: -8px;
  background: #4CAF50;
  color: white;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.7rem;
  font-weight: 700;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.3);
  animation: checkmarkPop 0.3s ease;
}

@keyframes pulse {

  0%,
  100% {
    box-shadow: 0 12px 35px rgba(0, 0, 0, 0.25);
  }

  50% {
    box-shadow: 0 15px 45px rgba(102, 126, 234, 0.4);
  }
}

@keyframes checkmarkPop {
  0% {
    transform: scale(0);
  }

  50% {
    transform: scale(1.2);
  }

  100% {
    transform: scale(1);
  }
}

/* 솔루션 기능 섹션 */
.solution-features {
  padding: 60px;
}

.solution-features h4 {
  font-size: 2rem;
  font-weight: 700;
  text-align: center;
  margin-bottom: 40px;
  color: #333;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 30px;
}

.feature-item {
  padding: 30px;
  border-radius: 15px;
  border: 1px solid #f0f0f0;
  transition: transform 0.3s ease, box-shadow 0.3s ease, border-color 0.3s ease;
  background: white;
}

.feature-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.15);
  border-color: #667eea;
}

.feature-icon {
  width: 60px;
  height: 60px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  margin-bottom: 20px;
  color: white;
}

.feature-item h5 {
  font-size: 1.3rem;
  font-weight: 700;
  margin-bottom: 15px;
  color: #333;
}

.feature-item p {
  color: #666;
  line-height: 1.6;
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

/* 모바일 반응형 */
@media (max-width: 768px) {
  .solution-hero {
    grid-template-columns: 1fr;
    text-align: center;
    padding: 40px 30px;
    gap: 40px;
  }

  .solution-hero h3 {
    font-size: 2.2rem;
  }

  .solution-tagline {
    font-size: 1.2rem;
  }

  .solution-features {
    padding: 40px 30px;
  }

  .features-grid {
    grid-template-columns: 1fr;
    gap: 25px;
  }

  /* 모바일에서 세로 배치 */
  .data-flow {
    flex-direction: column;
    gap: 15px;
  }

  .flow-item {
    flex-direction: column;
    gap: 15px;
  }

  .data-step {
    min-width: 120px;
    padding: 12px 25px;
  }

  .data-arrow {
    font-size: 1.3rem;
  }

  .arrow-horizontal {
    display: none;
  }

  .arrow-vertical {
    display: inline;
  }

  .data-arrow.active {
    transform: scale(1.1);
  }
}
</style>