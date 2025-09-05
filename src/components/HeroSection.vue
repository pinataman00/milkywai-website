<template>
  <section class="hero" id="home">
    <div class="hero-content">
      <h1>클라우드·데이터·AI로 여는 비즈니스 혁신</h1>
      <p>혁신적인 기술과 전문성을 바탕으로 고객의 경쟁력을 높이는 최적의 솔루션을 제공합니다</p>
      <div class="services-preview">
        <div class="service-tag" @click="handleServiceClick('cloudwai')">Cloud Service</div>
        <div class="service-tag" @click="handleServiceClick('datawai')">Data Service</div>
        <div class="service-tag" @click="handleServiceClick('dapq')">AI Service</div>
        <div class="service-tag" @click="handleServiceClick('kubesync')">DevOps Service</div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'HeroSection',
  emits: ['go-to-solution'],
  methods: {
    handleServiceClick(solutionType) {
      this.$emit('go-to-solution', solutionType)
    }
  }
}
</script>

<style scoped>
.hero {
  /* background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); */
  background: url('../assets/hero_bg2.jpg') center/cover no-repeat;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
}

/* 어둡게 오버레이 (콘텐츠 클릭 막지 않도록) */
.hero::after {
  content: "";
  position: absolute; inset: 0;
  background: rgba(0,0,0,.4);
  pointer-events: none;
}

.hero::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><defs><pattern id="grain" width="100" height="100" patternUnits="userSpaceOnUse"><circle cx="10" cy="10" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="30" cy="25" r="1.5" fill="rgba(255,255,255,0.05)"/><circle cx="60" cy="15" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="80" cy="40" r="1.5" fill="rgba(255,255,255,0.05)"/><circle cx="20" cy="60" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="45" cy="75" r="1.5" fill="rgba(255,255,255,0.05)"/><circle cx="70" cy="85" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="90" cy="70" r="1.5" fill="rgba(255,255,255,0.05)"/></pattern></defs><rect width="100" height="100" fill="url(%23grain)"/></svg>') repeat;
}

/* .hero-content {
  text-align: center;
  color: white;
  max-width: 800px;
  padding: 0 20px;
  position: relative;
  z-index: 2;
} */

/* .hero h1 {
  font-size: 3.5rem;
  font-weight: 700;
  margin-bottom: 20px;
  opacity: 0;
  transform: translateY(30px);
  animation: fadeInUp 1s ease forwards;
} */
.hero-content {
  position: relative; z-index: 1;
  color: #fff;
  max-width: min(1100px, 96vw);
  padding: 0 20px;
  text-align: center;
  display: flex;
  flex-direction: column;   /* h1, p 세로로 쌓기 */
  align-items: center;      /* 가로축 중앙 */
  justify-content: center;  /* 세로축 중앙 */
}

.hero h1 {
  white-space: nowrap;       /* 줄바꿈 금지 */
  word-break: keep-all;      /* 한국어 임의 분리 방지 */
  font-size: clamp(1.8rem, 5.2vw, 3rem);
  line-height: 1.2;
  font-weight: 700;
  margin-bottom: 20px;
  opacity: 0;
  transform: translateY(30px);
  animation: fadeInUp 1s ease forwards;
}


.hero p {
  font-size: 1.3rem;
  margin-bottom: 40px;
  opacity: 0;
  transform: translateY(30px);
  animation: fadeInUp 1s ease .2s forwards;
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
  background: rgba(255, 255, 255, 0.2);
  padding: 12px 24px;
  border-radius: 30px;
  font-weight: 500;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  transition: all 0.3s ease;
  cursor: pointer;
  user-select: none;
}

.service-tag:hover {
  transform: translateY(-2px);
  background: rgba(255, 255, 255, 0.3);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
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

@media (max-width: 768px) {
  .hero h1 {
    font-size: 2.5rem;
  }

  .hero p {
    font-size: 1.1rem;
  }

  .services-preview {
    flex-direction: column;
    align-items: center;
  }
}
</style>