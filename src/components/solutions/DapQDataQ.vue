<template>
  <div class="solution-detail">
    <div class="solution-hero">
      <div class="solution-hero-content">
        <div class="solution-badge">AI-Powered Analytics</div>
        <h3>DapQ & DataQ</h3>
        <p class="solution-tagline">생성형 AI 기반 데이터 분석 및 질의응답 솔루션</p>
        <p class="solution-description">기업 내부 데이터를 활용한 지능형 채팅 서비스(DapQ)와 자연어를 SQL로 변환하는 데이터 분석 플랫폼(DataQ)을 제공합니다.</p>
      </div>
      <div class="solution-hero-visual">
        <div class="ai-diagram">
          <div class="ai-icon">🤖</div>
          <div class="ai-flow" ref="aiFlowContainer">
            <div v-for="(step, index) in aiSteps" :key="step.id" class="flow-item" :ref="`flowItem${index}`">
              <div class="ai-step" :class="{
                active: step.active,
                completed: step.completed,
                'step-visible': isFlowVisible
              }" @mouseenter="handleStepHover(index, true)" @mouseleave="handleStepHover(index, false)">
                {{ step.name }}
              </div>
              <div v-if="index < aiSteps.length - 1" class="ai-arrow" :class="{
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
      <h4>AI 솔루션 특징</h4>
      <div class="features-grid">
        <div class="feature-item" v-for="feature in features" :key="feature.id">
          <div class="feature-icon">{{ feature.icon }}</div>
          <h5>{{ feature.title }}</h5>
          <p v-for="desc in feature.descriptions" :key="desc">{{ desc }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DapQDataQ',
  data() {
    return {
      isFlowVisible: false,
      animationTimer: null,
      aiSteps: [
        { id: 1, name: '질의', active: false, completed: false },
        { id: 2, name: 'AI 분석', active: false, completed: false },
        { id: 3, name: '답변', active: false, completed: false }
      ],
      features: [
        {
          id: 1,
          icon: '💬',
          title: 'DapQ - AI Chat',
          descriptions: [
            '기업 전용 지식 기반의 AI 챗봇',
            '24/7 즉각적이고 정확한 응답을 제공합니다'
          ]
        },
        {
          id: 2,
          icon: '🔍',
          title: 'DataQ - SQL 자동생성',
          descriptions: [
            '자연어 질의를 고도화된 SQL로 자동 변환하여',
            '누구나 빠르고 정밀한 데이터 분석을 할 수 있습니다'
          ]
        },
        {
          id: 3,
          icon: '📚',
          title: 'RAG 기반 검색',
          descriptions: [
            'Retrieval-Augmented Generation 기술로',
            '맥락에 맞고 신뢰성 높은 답변을 제공합니다'
          ]
        },
        {
          id: 4,
          icon: '🎯',
          title: '맞춤형 학습',
          descriptions: [
            '기업 고유의 데이터와 프로세스를 반영한 맞춤형 AI 모델로',
            '차별화된 경쟁력을 확보하세요'
          ]
        }
      ]
    }
  },
  mounted() {
    // 즉시 플로우 표시
    this.showAiFlow();

    // 1초 후 데모 애니메이션 시작
    setTimeout(() => {
      this.startAiDemo();
    }, 1000);
  },
  beforeUnmount() {
    // 컴포넌트 해제 시 타이머 정리
    if (this.animationTimer) {
      clearTimeout(this.animationTimer);
    }
  },
  methods: {
    showAiFlow() {
      // 플로우 요소들을 즉시 표시
      this.isFlowVisible = true;
    },

    handleStepHover(index, isHover) {
      if (isHover) {
        console.log(`AI Step ${index + 1} hovered: ${this.aiSteps[index].name}`);
      }
    },

    startAiDemo() {
      // 기존 애니메이션 정리
      this.resetAiFlow();

      // 단계별 활성화 애니메이션
      this.aiSteps.forEach((step, index) => {
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
      const totalDuration = this.aiSteps.length * 900 + 800 + 2000;
      this.animationTimer = setTimeout(() => {
        this.startAiDemo(); // 무한 반복
      }, totalDuration);
    },

    resetAiFlow() {
      // 타이머 정리
      if (this.animationTimer) {
        clearTimeout(this.animationTimer);
      }

      // AI 플로우 상태 초기화
      this.aiSteps.forEach(step => {
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

.ai-diagram {
  text-align: center;
  position: relative;
  width: 100%;
}

.ai-icon {
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

/* ✅ 수정된 AI Flow 스타일 */
.ai-flow {
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
.ai-step {
  background: rgba(255, 255, 255, 0.2);
  padding: 15px 25px;
  border-radius: 25px;
  font-size: 1rem;
  font-weight: 600;
  backdrop-filter: blur(5px);
  border: 2px solid rgba(255, 255, 255, 0.3);
  min-width: 90px;
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
.ai-step.step-visible {
  opacity: 1;
  animation: stepSlideIn 0.6s ease forwards;
}

.ai-step:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: translateY(-3px) scale(1.05);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.ai-arrow {
  font-size: 1.5rem;
  font-weight: bold;
  color: rgba(255, 255, 255, 0.6);
  transition: all 0.3s ease;
  /* ✅ 초기에도 보이도록 변경 */
  opacity: 0.7;
  transform: scale(1);
}

/* ✅ 보임 상태 클래스 추가 */
.ai-arrow.arrow-visible {
  opacity: 1;
  animation: arrowSlideIn 0.4s ease forwards;
}

.arrow-horizontal {
  display: inline;
}

.arrow-vertical {
  display: none;
}

.ai-arrow.active {
  color: rgba(76, 175, 80, 0.9);
  transform: scale(1.2);
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
.ai-step.active {
  background: rgba(255, 255, 255, 0.4);
  border-color: rgba(255, 255, 255, 0.7);
  transform: translateY(-3px) scale(1.08);
  box-shadow: 0 12px 35px rgba(0, 0, 0, 0.25);
  animation: pulse 1s ease-in-out infinite;
}

.ai-step.completed {
  background: rgba(76, 175, 80, 0.3);
  border-color: rgba(76, 175, 80, 0.7);
}

.ai-step.completed::after {
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
  margin-bottom: 8px;
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
  .ai-flow {
    flex-direction: column;
    gap: 15px;
  }

  .flow-item {
    flex-direction: column;
    gap: 15px;
  }

  .ai-step {
    min-width: 100px;
    padding: 12px 20px;
  }

  .ai-arrow {
    font-size: 1.3rem;
  }

  .arrow-horizontal {
    display: none;
  }

  .arrow-vertical {
    display: inline;
  }

  .ai-arrow.active {
    transform: scale(1.2);
  }
}
</style>