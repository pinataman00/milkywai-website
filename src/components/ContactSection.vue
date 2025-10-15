<template>
  <section class="section contact-section" id="contact">
    <div class="container">
      <img :src="logo" alt="milkywai logo" class="logo-img" />
      <h2 class="section-title contact-title">Contact Us</h2>

      <p class="section-subtitle">언제든지 문의해 주세요. 친절히 상담해 드리겠습니다</p>

      <div class="contact-info">
        <div class="contact-item">
          <font-awesome-icon :icon="['fas', 'location-dot']" class="contact-icon" />
          <h3>주소</h3>
          <p>서울시 서초구 강남대로51길 10<br>B1층 106-80호</p>
        </div>

        <div class="contact-item">
          <font-awesome-icon :icon="['fas', 'phone']" class="contact-icon" />
          <h3>전화</h3>
          <p>02-6672-3700</p>
        </div>

        <div class="contact-item">
          <font-awesome-icon :icon="['fas', 'globe']" class="contact-icon" />
          <h3>웹사이트</h3>
          <p>www.milkywai.co.kr</p>
        </div>
      </div>
      <div class="copyright-info">
        <p>
          Copyright (c) Milkywai Co.Ltd. All rights reserved.
        </p>
      </div>
    </div>
  </section>
</template>

<script>
// import logo from '../assets/milkywai_compound_CI.svg'
import logo from '../assets/favicon.svg'

export default {
  name: 'ContactSection',
  setup(){
    return {
      logo,
    }
  }


}
</script>

<style scoped>
.logo-img {
  height: 100px;
  /* display: block; */
}

.contact-title {
  color: white;
}
</style>