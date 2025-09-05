<template>
  <div class="solution-detail">
    <div class="solution-hero">
      <div class="solution-hero-content">
        <div class="solution-badge">Container Management</div>
        <h3>KubeSync</h3>
        <p class="solution-tagline">MSA와 컨테이너 환경을 위한 통합 관리 솔루션</p>
        <p class="solution-description">Kubernetes 환경에서 MSA 도입과 컨테이너들을 효율적으로 관리하며, CI/CD부터 모니터링까지 완전 자동화된 DevOps 환경을 제공합니다.</p>
      </div>
      <div class="solution-hero-visual">
        <div class="kube-diagram">
          <div class="kube-icon">⚙️</div>
          <div class="kube-features">
            <div class="kube-item">Container Orchestration</div>
            <div class="kube-item">CI/CD Automation</div>
            <div class="kube-item">Observability</div>
          </div>
        </div>
      </div>
    </div>

    <div class="solution-features">
      <h4>핵심 역량</h4>
      <div class="features-grid">
        <div class="feature-item">
          <div class="feature-icon">🔧</div>
          <h5>컨테이너 오케스트레이션</h5>
          <p>컨테이너 관리 및 효율적인 리소스 운영으로 안정적인 서비스 제공</p>
        </div>
        <div class="feature-item">
          <div class="feature-icon">🔄</div>
          <h5>자동화 파이프라인</h5>
          <p>애플리케이션 빌드부터 배포까지 완전 자동화된 CI/CD 파이프라인 구축</p>
        </div>
        <div class="feature-item">
          <div class="feature-icon">👁️</div>
          <h5>관측 가능성</h5>
          <p>Log, Metrics, Traces 수집을 통한 강력한 모니터링 및 장애 예방</p>
        </div>
        <div class="feature-item">
          <div class="feature-icon">📈</div>
          <h5>Auto Scaling</h5>
          <p>서비스 부하에 따른 자동 스케일링으로 최적의 성능과 비용 효율성 달성</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'KubeSync'
}
</script>

<style scoped>
/* 컴포넌트 특화 스타일이 필요한 경우 추가 */
</style>