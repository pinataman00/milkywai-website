<template>
  <section class="section solutions-section" id="solutions">
    <div class="container">
      <h2 class="section-title">Our Solutions</h2>
      <p class="section-subtitle">혁신적인 기술을 바탕으로 다양한 솔루션을 제공합니다</p>
      
      <!-- Solution Navigation -->
      <div class="solution-nav">
        <button 
          class="solution-nav-btn" 
          :class="{ active: activeSolution === 'cloudwai' }" 
          @click="changeSolution('cloudwai')"
        >
          CloudWai
        </button>
        <button 
          class="solution-nav-btn" 
          :class="{ active: activeSolution === 'kubesync' }" 
          @click="changeSolution('kubesync')"
        >
          KubeSync
        </button>
        <button 
          class="solution-nav-btn" 
          :class="{ active: activeSolution === 'dapq' }" 
          @click="changeSolution('dapq')"
        >
          DapQ & DataQ
        </button>
        <button 
          class="solution-nav-btn" 
          :class="{ active: activeSolution === 'neoflow' }" 
          @click="changeSolution('neoflow')"
        >
          NeoFlow
        </button>
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
}

.solution-nav-btn:hover,
.solution-nav-btn.active {
  background: #667eea;
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
}

@media (max-width: 768px) {
  .solution-nav {
    flex-direction: column;
    align-items: center;
    gap: 15px;
  }

  .solution-nav-btn {
    width: 80%;
    max-width: 300px;
  }
}
</style>