<template>
  <div class="solution-detail">
    <div class="solution-hero">
      <div class="solution-hero-content">
        <div class="solution-badge">AI Chat Service</div>
        <h3>Verora</h3>
        <p class="solution-tagline">생성형 AI 기반 지능형 채팅 서비스</p>
        <p class="solution-description">RAG(Retrieval-Augmented Generation) 기술로 기업 내부 데이터를 학습하여 정확하고 맞춤형 답변을 제공하는 24/7 AI 고객 지원 솔루션입니다.</p>
      </div>
      <div class="solution-hero-visual">
        <div class="ai-diagram">
          <div class="chatbot-lineart-animation">
            <svg width="320" height="240" viewBox="0 0 320 240" class="chatbot-svg">
              <!-- Chatbot Head - Line Art Style -->
              <g class="chatbot-head">
                <!-- Main head circle -->
                <circle cx="160" cy="90" r="50" fill="rgba(255,255,255,0.1)" stroke="white" stroke-width="4" class="bot-head"/>

                <!-- Eyes (properly positioned inside head) -->
                <circle cx="145" cy="85" r="7" fill="white" stroke="white" stroke-width="2" class="eye left-eye"/>
                <circle cx="175" cy="85" r="7" fill="white" stroke="white" stroke-width="2" class="eye right-eye"/>
                <circle cx="145" cy="85" r="3.5" fill="#333" class="pupil left-pupil"/>
                <circle cx="175" cy="85" r="3.5" fill="#333" class="pupil right-pupil"/>
                <!-- Eye highlights -->
                <circle cx="147" cy="83" r="1.5" fill="white" opacity="0.8" class="eye-highlight"/>
                <circle cx="177" cy="83" r="1.5" fill="white" opacity="0.8" class="eye-highlight"/>

                <!-- Mouth (properly positioned inside head) -->
                <path d="M 148 100 Q 160 108 172 100" stroke="white" stroke-width="3" fill="none" class="mouth" stroke-linecap="round"/>

                <!-- Antenna -->
                <line x1="160" y1="40" x2="160" y2="20" stroke="white" stroke-width="4" stroke-linecap="round"/>
                <circle cx="160" cy="15" r="4" fill="white" class="antenna-tip">
                  <animate attributeName="opacity" values="0.5;1;0.5" dur="2s" repeatCount="indefinite"/>
                </circle>
              </g>

              <!-- Speech Bubbles -->
              <g class="speech-bubbles">
                <!-- User message bubble -->
                <g class="bubble user-bubble" opacity="0.8">
                  <ellipse cx="70" cy="60" rx="35" ry="20" fill="white" stroke="#667eea" stroke-width="2"/>
                  <path d="M 95 70 L 110 80 L 95 75 Z" fill="white" stroke="#667eea" stroke-width="2"/>
                  <text x="70" y="65" text-anchor="middle" font-size="12" fill="#667eea" font-weight="600">안녕하세요!</text>
                </g>

                <!-- Bot response bubble -->
                <g class="bubble bot-bubble" opacity="0.8">
                  <ellipse cx="250" cy="120" rx="40" ry="25" fill="white" stroke="#4CAF50" stroke-width="2"/>
                  <path d="M 225 110 L 210 100 L 225 105 Z" fill="white" stroke="#4CAF50" stroke-width="2"/>
                  <text x="250" y="118" text-anchor="middle" font-size="11" fill="#4CAF50" font-weight="600">네, 도와드릴게요!</text>
                  <text x="250" y="130" text-anchor="middle" font-size="10" fill="#4CAF50">😊</text>
                </g>
              </g>

              <!-- Chat Message Lines -->
              <g class="chat-lines">
                <!-- Message Line 1 -->
                <g class="message-line" :class="{ active: currentBubble === 'user' }">
                  <line x1="220" y1="60" x2="290" y2="60" stroke="white" stroke-width="2" stroke-linecap="round"/>
                  <line x1="220" y1="68" x2="275" y2="68" stroke="white" stroke-width="2" stroke-linecap="round"/>
                  <circle cx="210" cy="64" r="3" fill="white"/>
                </g>

                <!-- Message Line 2 -->
                <g class="message-line" :class="{ active: currentBubble === 'bot' }">
                  <line x1="30" y1="130" x2="100" y2="130" stroke="white" stroke-width="2" stroke-linecap="round"/>
                  <line x1="30" y1="138" x2="90" y2="138" stroke="white" stroke-width="2" stroke-linecap="round"/>
                  <line x1="30" y1="146" x2="95" y2="146" stroke="white" stroke-width="2" stroke-linecap="round"/>
                  <circle cx="110" cy="138" r="3" fill="white"/>
                </g>

                <!-- Connecting lines -->
                <path d="M 210 90 Q 185 110 120 130" stroke="white" stroke-width="1" fill="none" stroke-dasharray="3,2" class="connection-line"/>
              </g>

              <!-- Floating indicators -->
              <g class="indicators">
                <circle cx="70" cy="180" r="2" fill="white" opacity="0.6" class="indicator">
                  <animate attributeName="cy" values="180;170;180" dur="2s" repeatCount="indefinite"/>
                  <animate attributeName="opacity" values="0.3;0.8;0.3" dur="2s" repeatCount="indefinite"/>
                </circle>
                <circle cx="250" cy="175" r="2" fill="white" opacity="0.4" class="indicator">
                  <animate attributeName="cy" values="175;165;175" dur="2.3s" repeatCount="indefinite"/>
                  <animate attributeName="opacity" values="0.2;0.6;0.2" dur="2.3s" repeatCount="indefinite"/>
                </circle>
                <circle cx="160" cy="190" r="2" fill="white" opacity="0.5" class="indicator">
                  <animate attributeName="cy" values="190;180;190" dur="2.7s" repeatCount="indefinite"/>
                  <animate attributeName="opacity" values="0.4;0.7;0.4" dur="2.7s" repeatCount="indefinite"/>
                </circle>
              </g>
            </svg>
          </div>

          <div class="ai-flow" ref="aiFlowContainer">
            <div v-for="(step, index) in veroraSteps" :key="step.id" class="flow-item" :ref="`flowItem${index}`">
              <div class="ai-step" :class="{
                active: step.active,
                completed: step.completed,
                'step-visible': isFlowVisible
              }" @mouseenter="handleStepHover(index, true)" @mouseleave="handleStepHover(index, false)">
                {{ step.name }}
              </div>
              <div v-if="index < veroraSteps.length - 1" class="ai-arrow" :class="{
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
      <h4>Verora 핵심 기능</h4>
      <div class="features-grid">
        <div class="feature-item" v-for="feature in features" :key="feature.id">
          <div class="feature-icon">{{ feature.icon }}</div>
          <h5>{{ feature.title }}</h5>
          <p v-for="desc in feature.descriptions" :key="desc">{{ desc }}</p>
        </div>
      </div>
    </div>

    <div class="chat-demo-section">
      <h4>실시간 채팅 데모</h4>
      <div class="chat-container">
        <div class="chat-header">
          <div class="chat-title">
            <span class="chat-icon">💬</span>
            Verora AI Assistant
            <span class="online-status">● 온라인</span>
          </div>
        </div>
        <div class="chat-messages" ref="chatMessages">
          <div 
            v-for="(message, index) in visibleMessages" 
            :key="index"
            class="message"
            :class="{ 'user-message': message.isUser, 'ai-message': !message.isUser }"
          >
            <div class="message-content">
              <div v-if="!message.isUser" class="ai-avatar">🤖</div>
              <div class="message-bubble">
                <span v-if="message.isTyping" class="typing-indicator">
                  <span></span><span></span><span></span>
                </span>
                <span v-else>{{ message.text }}</span>
              </div>
              <div v-if="message.isUser" class="user-avatar">👤</div>
            </div>
          </div>
        </div>
        <div class="chat-input">
          <input type="text" placeholder="메시지를 입력하세요..." readonly>
          <button>전송</button>
        </div>
      </div>
    </div>

    <div class="solution-architecture">
      <h4>Verora 아키텍처</h4>
      <div class="architecture-diagram">
        <div class="arch-layer" v-for="layer in architectureLayers" :key="layer.id">
          <h5>{{ layer.title }}</h5>
          <div class="arch-components">
            <div v-for="component in layer.components" :key="component" class="arch-component">
              {{ component }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Verora',
  data() {
    return {
      isFlowVisible: false,
      animationTimer: null,
      chatTimer: null,
      bubbleTimer: null,
      currentMessageIndex: 0,
      currentBubble: 'user',
      conversationTimer: null,
      visibleMessages: [],
      veroraSteps: [
        { id: 1, name: '사용자 질의', active: false, completed: false },
        { id: 2, name: 'RAG 검색', active: false, completed: false },
        { id: 3, name: 'AI 답변 생성', active: false, completed: false },
        { id: 4, name: '맞춤형 응답', active: false, completed: false }
      ],
      chatMessages: [
        { id: 1, text: "안녕하세요! 회사의 휴가 정책에 대해 알고 싶어요.", isUser: true },
        { id: 2, text: "네, 도와드리겠습니다! 휴가 정책 문서를 검색 중입니다...", isUser: false, isTyping: true },
        { id: 3, text: "회사의 휴가 정책에 따르면 연차는 입사 1년 후 15일이 부여되며, 반차 사용도 가능합니다. 국경일과 대체공휴일도 적용됩니다. 더 자세한 사항이 궁금하시면 언제든 말씀해 주세요!", isUser: false },
        { id: 4, text: "그럼 병가는 어떻게 되나요?", isUser: true },
        { id: 5, text: "병가는 연간 최대 30일까지 사용 가능하며, 의사 진단서 제출이 필요합니다. 3일 이상 연속 병가 시 반드시 진단서를 제출해야 하고, 병가 기간 중에는 기본급의 70%가 지급됩니다.", isUser: false }
      ],
      features: [
        {
          id: 1,
          icon: '💬',
          title: 'RAG 기반 검색',
          descriptions: [
            'Retrieval-Augmented Generation 기술로',
            '기업 내부 문서에서 정확한 정보를 검색하여 응답합니다'
          ]
        },
        {
          id: 2,
          icon: '📚',
          title: '다양한 문서 지원',
          descriptions: [
            'PDF, Word, Excel, PPT 등 다양한 문서 형식을',
            '자동으로 학습하여 지식베이스를 구축합니다'
          ]
        },
        {
          id: 3,
          icon: '🎯',
          title: '맞춤형 학습',
          descriptions: [
            '기업 고유의 데이터와 용어를 반영한',
            '맞춤형 AI 모델로 정확도를 극대화합니다'
          ]
        },
        {
          id: 4,
          icon: '⏰',
          title: '24/7 고객 지원',
          descriptions: [
            '언제든지 즉각적이고 정확한 응답으로',
            '고객 만족도를 향상시키고 운영 비용을 절감합니다'
          ]
        },
        {
          id: 5,
          icon: '🔄',
          title: '실시간 업데이트',
          descriptions: [
            '새로운 문서나 정보가 추가될 때마다',
            '자동으로 학습하여 최신 정보를 제공합니다'
          ]
        },
        {
          id: 6,
          icon: '🛡️',
          title: '보안 및 프라이버시',
          descriptions: [
            '기업 데이터의 보안을 보장하며',
            'On-premise 또는 Private Cloud 배포 지원'
          ]
        }
      ],
      architectureLayers: [
        {
          id: 1,
          title: 'Data Layer',
          components: ['문서 임베딩', 'Vector DB', '지식베이스']
        },
        {
          id: 2,
          title: 'AI Engine',
          components: ['RAG 검색', 'LLM 모델', 'Completion']
        },
        {
          id: 3,
          title: 'Interface',
          components: ['채팅 UI', 'API', 'Integration']
        }
      ]
    }
  },
  mounted() {
    this.showAiFlow();
    setTimeout(() => {
      this.startVeroraDemo();
      this.startConversationAnimation();
    }, 1000);
    setTimeout(() => {
      this.startChatDemo();
    }, 2000);
    setTimeout(() => {
      this.startBubbleAnimation();
    }, 500);
  },
  beforeUnmount() {
    if (this.animationTimer) {
      clearTimeout(this.animationTimer);
    }
    if (this.chatTimer) {
      clearTimeout(this.chatTimer);
    }
    if (this.bubbleTimer) {
      clearTimeout(this.bubbleTimer);
    }
  },
  methods: {
    startConversationAnimation() {
      const sequence = ['user', 'bot', 'user', 'bot'];
      let index = 0;

      const animate = () => {
        this.currentBubble = sequence[index];
        index = (index + 1) % sequence.length;

        this.conversationTimer = setTimeout(animate, 2500);
      };

      animate();
    },
    showAiFlow() {
      this.isFlowVisible = true;
    },

    handleStepHover(index, isHover) {
      if (isHover) {
        console.log(`Verora Step ${index + 1} hovered: ${this.veroraSteps[index].name}`);
      }
    },

    startVeroraDemo() {
      this.resetAiFlow();

      this.veroraSteps.forEach((step, index) => {
        this.animationTimer = setTimeout(() => {
          step.active = true;

          this.animationTimer = setTimeout(() => {
            step.active = false;
            step.completed = true;
          }, 800);
        }, index * 900);
      });

      const totalDuration = this.veroraSteps.length * 900 + 800 + 2000;
      this.animationTimer = setTimeout(() => {
        this.startVeroraDemo();
      }, totalDuration);
    },

    resetAiFlow() {
      if (this.animationTimer) {
        clearTimeout(this.animationTimer);
      }

      this.veroraSteps.forEach(step => {
        step.active = false;
        step.completed = false;
      });
    },

    startChatDemo() {
      this.currentMessageIndex = 0;
      this.visibleMessages = [];
      this.showNextMessage();
    },

    showNextMessage() {
      if (this.currentMessageIndex < this.chatMessages.length) {
        const message = { ...this.chatMessages[this.currentMessageIndex] };
        
        if (message.isTyping) {
          // 타이핑 인디케이터 먼저 보여주기
          this.visibleMessages.push(message);
          
          // 2초 후 실제 메시지로 교체
          this.chatTimer = setTimeout(() => {
            const updatedMessage = { ...message, isTyping: false };
            this.visibleMessages[this.visibleMessages.length - 1] = updatedMessage;
            
            // 다음 메시지 스케줄
            this.chatTimer = setTimeout(() => {
              this.currentMessageIndex++;
              this.showNextMessage();
            }, 1500);
          }, 2000);
        } else {
          this.visibleMessages.push(message);
          
          // 다음 메시지 스케줄
          this.chatTimer = setTimeout(() => {
            this.currentMessageIndex++;
            this.showNextMessage();
          }, message.isUser ? 2000 : 3000);
        }
      } else {
        // 모든 메시지 표시 완료 후 3초 대기 후 다시 시작
        this.chatTimer = setTimeout(() => {
          this.startChatDemo();
        }, 3000);
      }
    },

    startBubbleAnimation() {
      this.currentBubble = 1;

      // 각 말풍선을 순차적으로 표시
      this.bubbleTimer = setTimeout(() => {
        this.currentBubble = 2;

        this.bubbleTimer = setTimeout(() => {
          this.currentBubble = 3;

          this.bubbleTimer = setTimeout(() => {
            this.currentBubble = 0;

            // 3초 후 다시 시작
            this.bubbleTimer = setTimeout(() => {
              this.startBubbleAnimation();
            }, 3000);
          }, 2500);
        }, 2000);
      }, 1500);
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

/* AI 챗봇 라인아트 애니메이션 */
.chatbot-lineart-animation {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 30px;
  width: 100%;
}

.chatbot-svg {
  filter: drop-shadow(0 4px 15px rgba(255, 255, 255, 0.1));
  animation: gentle-float 4s ease-in-out infinite;
}

@keyframes gentle-float {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-6px); }
}

/* 라인아트 헤드 애니메이션 */
.chatbot-head {
  animation: subtle-nod 5s ease-in-out infinite;
  transform-origin: center bottom;
}

@keyframes subtle-nod {
  0%, 85%, 100% { transform: rotate(0deg); }
  90%, 95% { transform: rotate(1deg); }
}

/* 라인아트 눈 깜빡임 */
.eye {
  animation: line-blink 3s ease-in-out infinite;
}

@keyframes line-blink {
  0%, 90%, 100% { stroke-width: 1.5; }
  95% { stroke-width: 0.2; }
}

.pupil {
  animation: pupil-glow 3s ease-in-out infinite;
}

@keyframes pupil-glow {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.7; }
}

/* 메시지 라인 애니메이션 */
.message-line {
  opacity: 0.3;
  transition: all 0.4s ease;
}

.message-line.active {
  opacity: 1;
  animation: line-draw 0.8s ease;
}

@keyframes line-draw {
  0% {
    opacity: 0.3;
    stroke-dasharray: 50;
    stroke-dashoffset: 50;
  }
  100% {
    opacity: 1;
    stroke-dasharray: 0;
    stroke-dashoffset: 0;
  }
}

/* 연결선 애니메이션 */
.connection-line {
  opacity: 0.6;
  animation: dash-flow 3s ease-in-out infinite;
}

@keyframes dash-flow {
  0% { stroke-dashoffset: 0; }
  100% { stroke-dashoffset: -10; }
}

/* Particle positioning */
.particle1 { cx: 50; cy: 140; }
.particle2 { cx: 150; cy: 130; }
.particle3 { cx: 100; cy: 135; }

/* 픽사 스타일 눈 깜빡임 */
.eye {
  animation: pixarBlink 4s infinite;
  transform-origin: center;
}

@keyframes pixarBlink {
  0%, 90%, 100% { transform: scaleY(1); }
  92%, 94% { transform: scaleY(0.1); }
}

/* 친근한 동공 움직임 */
.pupil {
  animation: friendlyLook 5s infinite;
}

@keyframes friendlyLook {
  0%, 20% { transform: translate(0, 0); }
  25% { transform: translate(0.5px, -0.5px); }
  30%, 50% { transform: translate(-0.5px, 0); }
  55% { transform: translate(0.5px, 0.5px); }
  60%, 100% { transform: translate(0, 0); }
}

/* 눈 반짝임 효과 */
.eye-shine {
  animation: eyeSparkle 2s ease-in-out infinite;
}

@keyframes eyeSparkle {
  0%, 70%, 100% { opacity: 1; transform: scale(1); }
  75%, 85% { opacity: 0.7; transform: scale(1.2); }
}

/* 입 말하기 애니메이션 */
.mouth {
  animation: friendlyTalk 4s ease-in-out infinite;
}

@keyframes friendlyTalk {
  0%, 40% { d: path("M 70 95 Q 80 105 90 95"); }
  20% { d: path("M 70 95 Q 80 100 90 95"); }
  60%, 100% { d: path("M 70 95 Q 80 105 90 95"); }
}

/* 볼 홍조 애니메이션 */
.cheek {
  animation: blush 3s ease-in-out infinite;
}

.left-cheek { animation-delay: 0s; }
.right-cheek { animation-delay: 0.5s; }

@keyframes blush {
  0%, 80%, 100% { opacity: 0.6; transform: scale(1); }
  40% { opacity: 0.8; transform: scale(1.1); }
}

/* 안테나 불빛 */
.antenna-light {
  animation: friendlySensorPulse 2.5s ease-in-out infinite;
}

@keyframes friendlySensorPulse {
  0%, 100% { fill: #4FC3F7; opacity: 1; transform: scale(1); }
  50% { fill: #81D4FA; opacity: 0.8; transform: scale(1.15); }
}

/* 안테나 반짝임 */
.antenna-shine {
  animation: antennaShinePulse 2.5s ease-in-out infinite;
}

@keyframes antennaShinePulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; transform: scale(1.3); }
}

/* 하트 애니메이션 */
.heart {
  animation: heartbeat 4s ease-in-out infinite;
}

@keyframes heartbeat {
  0%, 70%, 100% { opacity: 0.3; transform: scale(1); }
  10%, 30% { opacity: 0.5; transform: scale(1.05); }
}

/* 말풍선 애니메이션 스타일 */
.speech-bubbles .bubble {
  opacity: 0;
  animation: bubbleFloat 6s ease-in-out infinite;
}

.speech-bubbles .user-bubble {
  animation-delay: 0s;
}

.speech-bubbles .bot-bubble {
  animation-delay: 3s;
}

@keyframes bubbleFloat {
  0%, 20% { opacity: 0; transform: scale(0.8) translateY(10px); }
  25%, 75% { opacity: 1; transform: scale(1) translateY(0); }
  80%, 100% { opacity: 0; transform: scale(0.8) translateY(-10px); }
}

/* 하얀색 말풍선 스타일 (챗봇 옆쪽 배치) */
.speech-bubbles {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 15px;
  z-index: 1;
  min-width: 200px;
}

.bubble {
  position: relative;
  background: white;
  color: #333;
  padding: 15px 20px;
  border-radius: 25px;
  font-size: 1rem;
  font-weight: 600;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
  opacity: 0;
  transform: translateX(20px) scale(0.8);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  white-space: nowrap;
  border: 2px solid #E3F2FD;
}

.bubble::before {
  content: '';
  position: absolute;
  left: -12px;
  top: 50%;
  transform: translateY(-50%);
  width: 0;
  height: 0;
  border-top: 12px solid transparent;
  border-right: 15px solid white;
  border-bottom: 12px solid transparent;
}

.bubble.active {
  opacity: 1;
  transform: translateX(0) scale(1);
  animation: bubbleSlideInRight 0.5s ease;
}

@keyframes bubbleSlideInRight {
  0% {
    opacity: 0;
    transform: translateX(20px) scale(0.8);
  }
  60% {
    transform: translateX(-5px) scale(1.05);
  }
  100% {
    opacity: 1;
    transform: translateX(0) scale(1);
  }
}

.ai-flow {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 15px;
  margin-top: 20px;
  min-height: 80px;
  flex-wrap: wrap;
}

.flow-item {
  display: flex;
  align-items: center;
  gap: 15px;
}

.ai-step {
  background: rgba(255, 255, 255, 0.2);
  padding: 12px 20px;
  border-radius: 25px;
  font-size: 0.9rem;
  font-weight: 600;
  backdrop-filter: blur(5px);
  border: 2px solid rgba(255, 255, 255, 0.3);
  min-width: 80px;
  text-align: center;
  transition: all 0.3s ease;
  cursor: pointer;
  position: relative;
  white-space: nowrap;
  opacity: 0.7;
  transform: translateY(0);
}

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
  font-size: 1.2rem;
  font-weight: bold;
  color: rgba(255, 255, 255, 0.6);
  transition: all 0.3s ease;
  opacity: 0.7;
  transform: scale(1);
}

.ai-arrow.arrow-visible {
  opacity: 1;
  animation: arrowSlideIn 0.4s ease forwards;
}

.arrow-horizontal { display: inline; }
.arrow-vertical { display: none; }

.ai-arrow.active {
  color: rgba(76, 175, 80, 0.9);
  transform: scale(1.2);
  text-shadow: 0 0 10px rgba(76, 175, 80, 0.5);
}

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
  width: 18px;
  height: 18px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.6rem;
  font-weight: 700;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.3);
  animation: checkmarkPop 0.3s ease;
}

@keyframes stepSlideIn {
  0% { opacity: 0.7; transform: translateY(10px); }
  100% { opacity: 1; transform: translateY(0); }
}

@keyframes arrowSlideIn {
  0% { opacity: 0.7; transform: scale(0.8); }
  100% { opacity: 1; transform: scale(1); }
}

@keyframes pulse {
  0%, 100% { box-shadow: 0 12px 35px rgba(0, 0, 0, 0.25); }
  50% { box-shadow: 0 15px 45px rgba(102, 126, 234, 0.4); }
}

@keyframes checkmarkPop {
  0% { transform: scale(0); }
  50% { transform: scale(1.2); }
  100% { transform: scale(1); }
}

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
  grid-template-columns: repeat(3, 1fr);
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

.solution-architecture {
  padding: 60px;
  background: #f8f9fa;
}

.solution-architecture h4 {
  font-size: 2rem;
  font-weight: 700;
  text-align: center;
  margin-bottom: 40px;
  color: #333;
}

.architecture-diagram {
  display: flex;
  justify-content: space-between;
  gap: 30px;
}

.arch-layer {
  flex: 1;
  background: white;
  border-radius: 15px;
  padding: 30px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.arch-layer h5 {
  font-size: 1.2rem;
  font-weight: 700;
  margin-bottom: 20px;
  color: #667eea;
}

.arch-components {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.arch-component {
  background: #f8f9fa;
  padding: 10px 15px;
  border-radius: 8px;
  font-size: 0.9rem;
  color: #555;
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

/* Chat Demo Styles */
.chat-demo-section {
  padding: 60px;
  background: #f8f9fa;
}

.chat-demo-section h4 {
  font-size: 2rem;
  font-weight: 700;
  text-align: center;
  margin-bottom: 40px;
  color: #333;
}

.chat-container {
  max-width: 600px;
  margin: 0 auto;
  background: white;
  border-radius: 20px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.chat-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 20px;
}

.chat-title {
  display: flex;
  align-items: center;
  gap: 10px;
  font-weight: 600;
  font-size: 1.1rem;
}

.chat-icon {
  font-size: 1.2rem;
}

.online-status {
  margin-left: auto;
  font-size: 0.9rem;
  color: #4CAF50;
}

.chat-messages {
  padding: 20px;
  min-height: 400px;
  max-height: 400px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.message {
  animation: messageSlideIn 0.5s ease-out;
}

.message-content {
  display: flex;
  align-items: flex-end;
  gap: 10px;
}

.user-message .message-content {
  justify-content: flex-end;
}

.ai-message .message-content {
  justify-content: flex-start;
}

.ai-avatar, .user-avatar {
  width: 35px;
  height: 35px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  flex-shrink: 0;
}

.ai-avatar {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.user-avatar {
  background: #f0f0f0;
}

.message-bubble {
  max-width: 70%;
  padding: 12px 16px;
  border-radius: 18px;
  line-height: 1.5;
  font-size: 0.95rem;
}

.user-message .message-bubble {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-bottom-right-radius: 6px;
}

.ai-message .message-bubble {
  background: #f8f9fa;
  color: #333;
  border-bottom-left-radius: 6px;
}

.typing-indicator {
  display: inline-block;
}

.typing-indicator span {
  display: inline-block;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #bbb;
  margin: 0 2px;
  animation: typingDots 1.4s infinite ease-in-out both;
}

.typing-indicator span:nth-child(1) {
  animation-delay: -0.32s;
}

.typing-indicator span:nth-child(2) {
  animation-delay: -0.16s;
}

.chat-input {
  padding: 20px;
  background: #f8f9fa;
  display: flex;
  gap: 10px;
  border-top: 1px solid #e9ecef;
}

.chat-input input {
  flex: 1;
  padding: 12px 16px;
  border: 1px solid #ddd;
  border-radius: 25px;
  outline: none;
  font-size: 0.95rem;
  background: white;
}

.chat-input button {
  padding: 12px 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 25px;
  font-weight: 600;
  cursor: pointer;
  transition: opacity 0.3s ease;
}

.chat-input button:hover {
  opacity: 0.9;
}

@keyframes messageSlideIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes typingDots {
  0%, 80%, 100% {
    transform: scale(0.8);
    opacity: 0.5;
  }
  40% {
    transform: scale(1);
    opacity: 1;
  }
}

@media (max-width: 968px) {
  .solution-hero {
    grid-template-columns: 1fr;
    text-align: center;
    padding: 40px 30px;
    gap: 40px;
  }

  .solution-hero h3 { font-size: 2.2rem; }
  .solution-tagline { font-size: 1.2rem; }
  
  .solution-features { padding: 40px 30px; }
  .features-grid { grid-template-columns: repeat(2, 1fr); gap: 25px; }

  .ai-flow { flex-direction: column; gap: 15px; }
  .flow-item { flex-direction: column; gap: 15px; }
  .ai-step { min-width: 100px; padding: 12px 20px; }
  
  .arrow-horizontal { display: none; }
  .arrow-vertical { display: inline; }
  
  .architecture-diagram { flex-direction: column; }
  
  .chat-demo-section { padding: 40px 20px; }
  .chat-container { margin: 0 10px; }
  .chat-messages { min-height: 300px; max-height: 300px; padding: 15px; }
  .message-bubble { max-width: 85%; font-size: 0.9rem; }
}

@media (max-width: 576px) {
  .features-grid { grid-template-columns: 1fr; }
  .chat-messages { min-height: 250px; max-height: 250px; }
}
</style>