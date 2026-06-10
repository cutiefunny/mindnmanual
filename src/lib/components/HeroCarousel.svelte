<script>
  import { onMount, onDestroy } from 'svelte';

  const slides = [
    {
      img: 'https://cdn.imweb.me/thumbnail/20230814/2620c7a0f6164.png',
      title: '스마트 심폐소생술 교육',
      subtitle: 'IoT 교구재와 시뮬레이션을 연동하여 실제와 같은 생생한 피드백을 제공합니다.',
      category: 'Signature Course'
    },
    {
      img: 'https://cdn.imweb.me/thumbnail/20230814/3287011933b31.png',
      title: '대한심폐소생협회 인증기관',
      subtitle: '공신력 있는 전문 교육 강사진과 과학적인 프로그램으로 정확한 응급처치를 전수합니다.',
      category: 'KACPR Course'
    },
    {
      img: 'https://cdn.imweb.me/thumbnail/20230814/9e30461089205.png',
      title: '교직원 응급처치 교육',
      subtitle: '교육기관 종사자를 위한 필수 법정 의무 연수로 교내 안전을 확실하게 지킵니다.',
      category: 'School Staff Course'
    },
    {
      img: 'https://cdn.imweb.me/thumbnail/20230814/d3987e7a7dc4f.png',
      title: '청소년 및 어린이 안전교육',
      subtitle: '눈높이에 맞춘 쉽고 흥미로운 실습으로 어린 학생들에게 생명의 가치를 가르칩니다.',
      category: 'Youth & Children'
    }
  ];

  let currentSlide = $state(0);
  let intervalId;

  function autoPlayNextSlide() {
    currentSlide = (currentSlide + 1) % slides.length;
  }

  function resetAutoplay() {
    if (intervalId) clearInterval(intervalId);
    intervalId = setInterval(autoPlayNextSlide, 5000);
  }

  function nextSlide() {
    currentSlide = (currentSlide + 1) % slides.length;
    resetAutoplay();
  }

  function prevSlide() {
    currentSlide = (currentSlide - 1 + slides.length) % slides.length;
    resetAutoplay();
  }

  function goToSlide(index) {
    currentSlide = index;
    resetAutoplay();
  }

  onMount(() => {
    intervalId = setInterval(autoPlayNextSlide, 5000);
  });

  onDestroy(() => {
    if (intervalId) clearInterval(intervalId);
  });
</script>

<div class="carousel-container">
  <!-- Slides -->
  {#each slides as slide, idx}
    <div 
      class="slide {idx === currentSlide ? 'active' : ''}" 
      style="background-image: linear-gradient(to right, rgba(0, 0, 0, 0.7) 30%, rgba(0, 0, 0, 0.2) 100%), url({slide.img});"
    >
      <div class="container slide-content">
        <div class="slide-text-wrapper">
          <span class="slide-category">{slide.category}</span>
          <h1 class="slide-title">{slide.title}</h1>
          <p class="slide-subtitle">{slide.subtitle}</p>
          <div class="slide-ctas">
            <a href="#programs" class="btn-primary-slide">교육 프로그램 보기</a>
            <a href="#brand" class="btn-outline-slide">브랜드 스토리</a>
          </div>
        </div>
      </div>
    </div>
  {/each}

  <!-- Navigation Arrows -->
  <button class="nav-arrow prev" onclick={prevSlide} aria-label="Previous slide">
    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M15 18l-6-6 6-6" stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
  </button>
  <button class="nav-arrow next" onclick={nextSlide} aria-label="Next slide">
    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M9 18l6-6-6-6" stroke-linecap="round" stroke-linejoin="round"/>
    </svg>
  </button>

  <!-- Dots Indicators -->
  <div class="dots-indicators">
    {#each slides as _, idx}
      <button 
        class="dot {idx === currentSlide ? 'active' : ''}" 
        onclick={() => goToSlide(idx)}
        aria-label="Go to slide {idx + 1}"
      ></button>
    {/each}
  </div>
</div>

<style>
  .carousel-container {
    position: relative;
    width: 100%;
    height: 100vh;
    min-height: 600px;
    overflow: hidden;
    background-color: #000;
  }

  .slide {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
    opacity: 0;
    visibility: hidden;
    transition: opacity 1s ease-in-out, visibility 1s ease-in-out;
    display: flex;
    align-items: center;
    z-index: 1;
  }

  .slide.active {
    opacity: 1;
    visibility: visible;
    z-index: 2;
  }

  .slide-content {
    color: #ffffff;
    z-index: 3;
    padding-top: 80px; /* Header spacing */
  }

  .slide-text-wrapper {
    max-width: 650px;
    transform: translateY(30px);
    opacity: 0;
    transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1) 0.3s;
  }

  .slide.active .slide-text-wrapper {
    transform: translateY(0);
    opacity: 1;
  }

  .slide-category {
    font-size: 14px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 2px;
    color: var(--primary);
    margin-bottom: 15px;
    display: inline-block;
  }

  .slide-title {
    font-size: 56px;
    font-weight: 700;
    line-height: 1.15;
    margin-bottom: 20px;
    word-break: keep-all;
  }

  .slide-subtitle {
    font-size: 18px;
    line-height: 1.6;
    margin-bottom: 40px;
    color: rgba(255, 255, 255, 0.85);
    word-break: keep-all;
  }

  .slide-ctas {
    display: flex;
    gap: 16px;
  }

  .btn-primary-slide {
    background-color: var(--primary);
    color: #fff;
    padding: 14px 28px;
    border-radius: 30px;
    font-weight: 600;
    font-size: 15px;
    box-shadow: 0 4px 15px rgba(255, 121, 0, 0.3);
  }

  .btn-primary-slide:hover {
    background-color: var(--primary-hover);
    transform: translateY(-2px);
  }

  .btn-outline-slide {
    background-color: transparent;
    color: #fff;
    border: 2px solid rgba(255, 255, 255, 0.6);
    padding: 12px 28px;
    border-radius: 30px;
    font-weight: 600;
    font-size: 15px;
  }

  .btn-outline-slide:hover {
    background-color: #ffffff;
    color: var(--dark);
    border-color: #ffffff;
    transform: translateY(-2px);
  }

  /* Navigation Arrows */
  .nav-arrow {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background-color: rgba(255, 255, 255, 0.1);
    color: #fff;
    border: none;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all var(--transition-speed);
    z-index: 10;
  }

  .nav-arrow:hover {
    background-color: var(--primary);
    color: #fff;
  }

  .nav-arrow.prev {
    left: 30px;
  }

  .nav-arrow.next {
    right: 30px;
  }

  /* Dot indicators at the bottom */
  .dots-indicators {
    position: absolute;
    bottom: 40px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 12px;
    z-index: 10;
  }

  .dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background-color: rgba(255, 255, 255, 0.4);
    border: none;
    cursor: pointer;
    transition: all var(--transition-speed);
  }

  .dot.active {
    background-color: var(--primary);
    width: 28px;
    border-radius: 5px;
  }

  /* Responsive styling */
  @media (max-width: 768px) {
    .carousel-container {
      height: 90vh;
      min-height: 500px;
    }

    .slide-title {
      font-size: 36px;
    }

    .slide-subtitle {
      font-size: 15px;
      margin-bottom: 30px;
    }

    .slide-ctas {
      flex-direction: column;
      gap: 12px;
      width: 100%;
    }

    .btn-primary-slide, .btn-outline-slide {
      text-align: center;
      width: 100%;
    }

    .nav-arrow {
      display: none;
    }
  }
</style>
