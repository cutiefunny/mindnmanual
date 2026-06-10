<script>
  import { onMount } from 'svelte';
  import Header from '$lib/components/Header.svelte';
  import Footer from '$lib/components/Footer.svelte';
  import FloatingActions from '$lib/components/FloatingActions.svelte';
  import Certifications from '$lib/components/Certifications.svelte';

  // Hero Carousel slider state
  let currentSlide = $state(0);
  const slides = [
    { url: 'https://cdn.imweb.me/thumbnail/20230905/162e147ae4643.jpg', alt: '어린이 안전 교육 실습 1' },
    { url: 'https://cdn.imweb.me/thumbnail/20230830/53ac9b66a2c6a.jpg', alt: '어린이 안전 교육 실습 2' },
    { url: 'https://cdn.imweb.me/thumbnail/20230830/d296b164cd752.jpg', alt: '어린이 안전 교육 실습 3' },
    { url: 'https://cdn.imweb.me/thumbnail/20230830/efcc2b8b672eb.jpg', alt: '어린이 안전 교육 실습 4' }
  ];

  // Carousel layout and responsive width tracking
  let containerWidth = $state(0);
  let slideWidth = $derived(Math.min(800, containerWidth * 0.7));
  let gap = 24;
  let translateX = $derived((containerWidth / 2) - (currentSlide * (slideWidth + gap)) - (slideWidth / 2));

  // Sticky sub-navigation state
  let stickyMenu = $state(false);
  let activeSection = $state('intro');

  const navItems = [
    { id: 'intro', name: '프로그램 소개' },
    { id: 'apply', name: '교육 신청' }
  ];

  let slideInterval;

  onMount(() => {
    // Background slide rotation
    slideInterval = setInterval(() => {
      currentSlide = (currentSlide + 1) % slides.length;
    }, 4500);

    // Scroll handler for sticky sub-nav and section active highlighting
    const handleScroll = () => {
      const scrollY = window.scrollY;
      stickyMenu = scrollY > 650; // Triggers sticky position below hero section

      for (const item of navItems) {
        const el = document.getElementById(item.id);
        if (el) {
          const rect = el.getBoundingClientRect();
          // Active if section top is near the top of viewport (offset by header + sub-nav height)
          if (rect.top <= 165 && rect.bottom >= 165) {
            activeSection = item.id;
            break;
          }
        }
      }
    };

    window.addEventListener('scroll', handleScroll);
    handleScroll(); // Initial check

    return () => {
      clearInterval(slideInterval);
      window.removeEventListener('scroll', handleScroll);
    };
  });

  function prevSlide() {
    clearInterval(slideInterval);
    currentSlide = (currentSlide - 1 + slides.length) % slides.length;
    slideInterval = setInterval(() => {
      currentSlide = (currentSlide + 1) % slides.length;
    }, 4500);
  }

  function nextSlide() {
    clearInterval(slideInterval);
    currentSlide = (currentSlide + 1) % slides.length;
    slideInterval = setInterval(() => {
      currentSlide = (currentSlide + 1) % slides.length;
    }, 4500);
  }

  function scrollToSection(id) {
    const el = document.getElementById(id);
    if (el) {
      const offset = 140; // sticky header (70px) + sticky subnav (60px) + safety padding
      const bodyRect = document.body.getBoundingClientRect().top;
      const elementRect = el.getBoundingClientRect().top;
      const elementPosition = elementRect - bodyRect;
      const offsetPosition = elementPosition - offset;

      window.scrollTo({
        top: offsetPosition,
        behavior: 'smooth'
      });
    }
  }
</script>

<svelte:head>
  <title>어린이 안전 교육 | 마인드앤매뉴얼</title>
  <meta name="description" content="어린이안전관리법 기준에 의거한 어린이 이용시설 종사자 대상 법정의무 안전교육과정. 전문 자격 보유 강사진이 시설로 직접 찾아가 실습 중심으로 교육합니다." />
</svelte:head>

<Header />

<!-- Page Hero Section -->
<section class="hero-section">
  <div class="container hero-container-header animate-fade-in">
    <div class="hero-header-row">
      <div class="hero-title-area">
        <h1 class="hero-title">어린이 안전 교육</h1>
        <p class="hero-subtitle">어린이 안전교육은 어린이 이용시설 종사자를 대상으로 매년 진행하는 법정의무 교육과정입니다.</p>
      </div>
      <div class="hero-badge-area">
        <div class="circular-badge">어린이 이용시설<br />종사자</div>
        <div class="circular-badge">출강</div>
      </div>
    </div>
  </div>

  <!-- Swiper style custom slide container -->
  <div class="carousel-container animate-fade-in" bind:clientWidth={containerWidth}>
    <div class="carousel-track" style="transform: translateX({translateX}px); gap: {gap}px;">
      {#each slides as slide, index}
        <div class="carousel-slide {index === currentSlide ? 'active' : ''}" style="width: {slideWidth}px;">
          <img src={slide.url} alt={slide.alt} />
        </div>
      {/each}
    </div>

    <!-- Navigation Arrows -->
    <button class="carousel-arrow prev" onclick={prevSlide} aria-label="이전 슬라이드">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
        <polyline points="15 18 9 12 15 6"></polyline>
      </svg>
    </button>
    <button class="carousel-arrow next" onclick={nextSlide} aria-label="다음 슬라이드">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
        <polyline points="9 18 15 12 9 6"></polyline>
      </svg>
    </button>
  </div>
</section>

<!-- Sticky Sub-Navigation -->
<div class="sub-nav-wrapper {stickyMenu ? 'sticky' : ''}">
  <div class="container sub-nav-container">
    <div class="sub-nav-pills">
      {#each navItems as item}
        <button 
          class="sub-nav-pill {activeSection === item.id ? 'active' : ''}"
          onclick={() => scrollToSection(item.id)}
        >
          {item.name}
        </button>
      {/each}
    </div>
  </div>
</div>

<!-- Section 1: 프로그램 소개 -->
<section id="intro" class="intro-section py-section">
  <div class="container section-container">
    <div class="section-header animate-slide-up">
      <h2 class="section-title">프로그램 소개</h2>
    </div>

    <div class="intro-desc-wrapper animate-slide-up">
      <div class="intro-desc-card">
        <ul class="intro-desc-list">
          <li>
            <span class="bullet-dot"></span>
            『어린이안전관리에 관한 법률』에 의거한 전문강사와 프로그램에 따라 시설에서 발생할 수 있는 응급처치법과 영∙유아를 포함한 심폐소생술을 학습할 수 있습니다.
          </li>
          <li>
            <span class="bullet-dot"></span>
            교육은 출장 강의 형태로 진행됩니다.
          </li>
          <li class="font-weight-500">
            <span class="bullet-dot"></span>
            어린이 안전교육은 관련법령 『어린이안전관리에 관한 법률 제16조(어린이 안전교육)』의 기준 등을 준수하여 진행됩니다.
          </li>
        </ul>
      </div>
    </div>

    <!-- 2x2 Split Cards Grid -->
    <div class="details-grid animate-slide-up">
      
      <!-- Card 1: 대상 / 비용 -->
      <div class="split-card">
        <div class="card-left-block">
          <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/6f92696af91cb.png" alt="" class="card-left-icon" />
          <h3 class="card-left-title">대상 / 비용</h3>
        </div>
        <div class="card-right-block">
          <ul class="body-list">
            <li><span class="bullet-label">대상 :</span> 어린이 이용시설 종사자</li>
            <li><span class="bullet-label">교육 비용 :</span> 300,000원 (VAT 별도)</li>
            <li><span class="bullet-label">이론/실습 별도 신청 시 :</span> 각 200,000원 (VAT 별도)</li>
          </ul>
        </div>
      </div>

      <!-- Card 2: 교육 내용 -->
      <div class="split-card">
        <div class="card-left-block">
          <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/b71f0441e2063.png" alt="" class="card-left-icon" />
          <h3 class="card-left-title">교육 내용</h3>
        </div>
        <div class="card-right-block flex-row-layout">
          <ul class="body-list check-list">
            <li>사고사례</li>
            <li>반응확인 및 신고절차</li>
            <li>성인/영아 가슴압박소생술</li>
            <li>자동심장충격기 사용법</li>
            <li>성인/영아 기도폐쇄 처치법</li>
          </ul>
          <div class="time-badge">교육 시간 : 총 4시간</div>
        </div>
      </div>

      <!-- Card 3: 강사 구성 -->
      <div class="split-card">
        <div class="card-left-block">
          <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/1efbaf3bafab4.png" alt="" class="card-left-icon" />
          <h3 class="card-left-title">강사 구성</h3>
        </div>
        <div class="card-right-block">
          <ul class="body-list">
            <li>대한심폐소생협회 강사</li>
            <li><span class="bullet-label">진행강사 :</span> 간호사(관련 자격 보유자) 또는 응급구조사 (1급: 경력 2년 이상, 2급: 경력 3년 이상 종사자)</li>
            <li><span class="bullet-label">보조강사 :</span> 교육생 20명 이상 시 추가 실습 보조 배치</li>
          </ul>
        </div>
      </div>

      <!-- Card 4: 실습 교구 (Double Layout) -->
      <div class="split-card double-layout-card">
        <div class="card-left-block">
          <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/6197b24ed1066.png" alt="" class="card-left-icon" />
          <h3 class="card-left-title">실습 교구</h3>
        </div>
        <div class="card-right-block flex-row no-padding overflow-hidden">
          <div class="card-text-side">
            <ul class="body-list check-list">
              <li>성인 마네킨</li>
              <li>영아 마네킨</li>
              <li>자동심장충격기(AED)</li>
              <li>기도폐쇄 조끼</li>
            </ul>
          </div>
          <div class="card-img-side">
            <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/fe3dff2faf3a0.png" alt="어린이 안전 실습 교구" />
          </div>
        </div>
      </div>

    </div>
  </div>
</section>

<!-- Section 2: 교육 신청 -->
<section id="apply" class="apply-section">
  <div class="apply-overlay"></div>
  <div class="container apply-content animate-slide-up">
    
    <div class="section-header text-center light-theme">
      <h2 class="section-title text-white">교육 신청</h2>
      <p class="apply-subheading">I 신청 방법 / 비용 납입</p>
    </div>

    <div class="apply-cards-grid">
      <!-- Card Left: 신청 방법 -->
      <div class="split-card apply-info-card">
        <div class="card-left-block">
          <svg class="apply-card-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <path d="M12 20h9"></path>
            <path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"></path>
          </svg>
          <span class="card-left-title">신청 방법</span>
        </div>
        <div class="card-right-block">
          <ul class="apply-steps-list">
            <li>
              <span class="step-badge">1</span>
              <span class="step-desc">하단의 <strong>[교육신청하기]</strong> 버튼을 클릭합니다.</span>
            </li>
            <li>
              <span class="step-badge">2</span>
              <span class="step-desc">신청 양식에 따라 상세하게 정보를 기재하여 신청을 접수합니다.</span>
            </li>
            <li>
              <span class="step-badge">3</span>
              <span class="step-desc">신청 완료 시 마인드앤매뉴얼 담당자 연락을 통해 교육과정 상담이 진행됩니다.</span>
            </li>
            <li>
              <span class="step-badge">4</span>
              <span class="step-desc">일정 등 신청 내용 변경 시 유선 상담을 통해 변경사항을 접수합니다.</span>
            </li>
          </ul>
        </div>
      </div>

      <!-- Card Right: 비용 납입 -->
      <div class="split-card apply-info-card">
        <div class="card-left-block">
          <svg class="apply-card-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <rect x="2" y="4" width="20" height="16" rx="2" ry="2"></rect>
            <line x1="12" y1="18" x2="12" y2="12"></line>
            <line x1="8" y1="15" x2="16" y2="15"></line>
            <path d="M12 2v2"></path>
            <path d="M12 20v2"></path>
            <path d="M4.93 4.93l1.41 1.41"></path>
            <path d="M17.66 17.66l1.41 1.41"></path>
            <path d="M2 12h2"></path>
            <path d="M20 12h2"></path>
            <path d="M6.34 17.66l-1.41 1.41"></path>
            <path d="M19.07 4.93l-1.41 1.41"></path>
          </svg>
          <span class="card-left-title">비용 납입</span>
        </div>
        <div class="card-right-block">
          <ul class="apply-payment-info">
            <li>
              <h4 class="payment-title">비용 납입 방법</h4>
              <p class="payment-text">- 교육 완료 및 수료 후 30일 이내에 납부합니다.</p>
              <p class="payment-text">- 세금계산서 발행 및 카드 결제가 가능합니다.</p>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <!-- CTA Trigger Button (Link to centralized inquiry /contact) -->
    <div class="cta-button-container text-center">
      <a class="cta-apply-btn" href="/contact">
        <span>교육신청하기</span>
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="cta-arrow">
          <line x1="5" y1="12" x2="19" y2="12"></line>
          <polyline points="12 5 19 12 12 19"></polyline>
        </svg>
      </a>
    </div>

  </div>
</section>

<Certifications />
<Footer />
<FloatingActions />

<style>
  /* Hero Section */
  .hero-section {
    padding-top: 130px;
    padding-bottom: 50px;
    background-color: #f6f3ed;
    overflow: hidden;
  }

  .hero-container-header {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
  }

  .hero-header-row {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    margin-bottom: 35px;
    gap: 40px;
  }

  .hero-title-area {
    flex: 1;
  }

  .hero-title {
    font-size: 38px;
    font-weight: 800;
    line-height: 1.35;
    color: var(--dark);
    word-break: keep-all;
  }

  .hero-subtitle {
    font-size: 16px;
    color: var(--text-muted);
    margin-top: 16px;
    word-break: keep-all;
    max-width: 700px;
  }

  .hero-badge-area {
    display: flex;
    gap: 15px;
    flex-shrink: 0;
  }

  .circular-badge {
    width: 95px;
    height: 95px;
    border-radius: 50%;
    background-color: var(--primary);
    color: #ffffff;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 13px;
    font-weight: 700;
    text-align: center;
    line-height: 1.35;
    box-shadow: 0 4px 15px rgba(255, 121, 0, 0.2);
  }

  /* Carousel Slider */
  .carousel-container {
    position: relative;
    width: 100%;
    margin-top: 20px;
    overflow: hidden;
    padding: 20px 0;
  }

  .carousel-track {
    display: flex;
    transition: transform 0.6s cubic-bezier(0.25, 0.8, 0.25, 1);
    will-change: transform;
  }

  .carousel-slide {
    flex-shrink: 0;
    height: 440px;
    border-radius: 24px;
    overflow: hidden;
    opacity: 0.5;
    transform: scale(0.93);
    transition: transform 0.6s ease, opacity 0.6s ease;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  }

  .carousel-slide.active {
    opacity: 1;
    transform: scale(1);
    box-shadow: 0 15px 40px rgba(0, 0, 0, 0.15);
  }

  .carousel-slide img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .carousel-arrow {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background-color: rgba(255, 255, 255, 0.9);
    border: none;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--dark);
    cursor: pointer;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    z-index: 10;
    transition: all 0.3s ease;
  }

  .carousel-arrow:hover {
    background-color: var(--primary);
    color: #ffffff;
    transform: translateY(-50%) scale(1.08);
  }

  .carousel-arrow.prev {
    left: calc((100% - 800px) / 4);
  }

  .carousel-arrow.next {
    right: calc((100% - 800px) / 4);
  }

  /* Sticky Sub-Navigation */
  .sub-nav-wrapper {
    width: 100%;
    background-color: var(--bg-white);
    border-bottom: 1px solid var(--border-color);
    z-index: 100;
    position: relative;
  }

  .sub-nav-wrapper.sticky {
    position: fixed;
    top: 70px; /* Header height offset */
    left: 0;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.05);
    animation: slideInDown 0.3s ease;
  }

  @keyframes slideInDown {
    from { transform: translateY(-100%); }
    to { transform: translateY(0); }
  }

  .sub-nav-container {
    max-width: 1200px;
    height: 60px;
    display: flex;
    align-items: center;
  }

  .sub-nav-pills {
    display: flex;
    gap: 8px;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
    width: 100%;
    padding: 5px 0;
  }

  .sub-nav-pills::-webkit-scrollbar {
    display: none;
  }

  .sub-nav-pill {
    padding: 10px 24px;
    border-radius: 30px;
    font-size: 15px;
    font-weight: 500;
    background-color: #f5f5f7;
    color: var(--text-muted);
    border: none;
    cursor: pointer;
    white-space: nowrap;
    transition: all var(--transition-speed);
  }

  .sub-nav-pill.active, .sub-nav-pill:hover {
    background-color: var(--primary);
    color: #ffffff;
  }

  /* Sections General */
  .section-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
  }

  .section-title {
    font-size: 30px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 30px;
    position: relative;
    padding-left: 14px;
  }

  .section-title::before {
    content: '';
    position: absolute;
    left: 0;
    top: 5px;
    bottom: 5px;
    width: 4px;
    background-color: var(--primary);
    border-radius: 2px;
  }

  /* Intro Section */
  .intro-desc-card {
    background-color: var(--bg-white);
    border-radius: 24px;
    padding: 40px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.02);
    border: 1px solid rgba(0, 0, 0, 0.03);
    margin-bottom: 30px;
  }

  .intro-desc-list {
    display: flex;
    flex-direction: column;
    gap: 22px;
  }

  .intro-desc-list li {
    font-size: 16px;
    line-height: 1.8;
    color: var(--text);
    position: relative;
    padding-left: 20px;
    word-break: keep-all;
  }

  .bullet-dot {
    position: absolute;
    left: 4px;
    top: 11px;
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background-color: var(--primary);
  }

  .font-weight-500 {
    font-weight: 500;
    color: var(--dark) !important;
  }

  /* Split Grid */
  .details-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
  }

  .split-card {
    display: flex;
    background-color: var(--bg-white);
    border-radius: 24px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.02);
    border: 1px solid rgba(0, 0, 0, 0.04);
    transition: transform var(--transition-speed), box-shadow var(--transition-speed);
  }

  .split-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 15px 45px rgba(0, 0, 0, 0.06);
  }

  .card-left-block {
    flex: 0 0 145px;
    background-color: var(--primary);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 15px;
    padding: 24px;
    color: #ffffff;
    text-align: center;
  }

  .card-left-icon {
    width: 36px;
    height: 36px;
    object-fit: contain;
    filter: brightness(0) invert(1);
  }

  .card-left-title {
    font-size: 15px;
    font-weight: 700;
    line-height: 1.35;
    word-break: keep-all;
  }

  .card-right-block {
    flex: 1;
    padding: 35px;
    background-color: var(--bg-white);
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .flex-row-layout {
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
    gap: 20px;
  }

  /* Lists Styling */
  .body-list {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .body-list li {
    font-size: 15px;
    color: var(--text);
    line-height: 1.6;
    word-break: keep-all;
  }

  .bullet-label {
    font-weight: 600;
    color: var(--dark);
    margin-right: 5px;
  }

  .notice-item {
    font-size: 13.5px !important;
    color: var(--primary) !important;
    font-weight: 500;
    margin-top: 4px;
  }

  .check-list li {
    position: relative;
    padding-left: 20px;
  }

  .check-list li::before {
    content: '✓';
    position: absolute;
    left: 0;
    color: var(--primary);
    font-weight: bold;
  }

  .time-badge {
    padding: 10px 18px;
    border-radius: 30px;
    background-color: rgba(255, 121, 0, 0.08);
    color: var(--primary);
    font-weight: 700;
    font-size: 14px;
    white-space: nowrap;
    text-align: center;
  }

  /* Double Layout Card */
  .double-layout-card {
    padding: 0;
  }

  .double-layout-card .card-right-block.flex-row {
    flex-direction: row;
    padding: 0;
  }

  .card-text-side {
    flex: 1.1;
    padding: 35px;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .card-img-side {
    flex: 0.9;
    height: 100%;
    overflow: hidden;
  }

  .card-img-side img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .no-padding {
    padding: 0 !important;
  }

  /* Apply Section */
  .apply-section {
    position: relative;
    padding: 90px 0;
    background-image: linear-gradient(rgba(0, 0, 0, 0.75), rgba(0, 0, 0, 0.75)), url('https://cdn.imweb.me/thumbnail/20230908/70bfb0744d5bf.png');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    background-attachment: fixed;
    color: #ffffff;
    overflow: hidden;
  }

  .apply-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.2);
    z-index: 1;
  }

  .apply-content {
    position: relative;
    z-index: 2;
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
  }

  .apply-subheading {
    font-size: 20px;
    font-weight: 600;
    color: #ffffff;
    margin-bottom: 40px;
    margin-top: -15px;
  }

  .apply-cards-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
  }

  .apply-info-card {
    background-color: rgba(255, 255, 255, 0.98);
  }

  .apply-info-card .card-right-block {
    background-color: transparent;
  }

  .apply-card-icon {
    width: 38px;
    height: 38px;
    stroke-width: 2.2;
  }

  /* Apply Steps List */
  .apply-steps-list {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .apply-steps-list li {
    display: flex;
    align-items: flex-start;
    gap: 12px;
  }

  .step-badge {
    flex-shrink: 0;
    width: 22px;
    height: 22px;
    border-radius: 50%;
    background-color: var(--primary);
    color: #ffffff;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 12px;
    font-weight: 700;
    margin-top: 2px;
  }

  .step-desc {
    font-size: 14px;
    color: var(--text);
    line-height: 1.6;
    word-break: keep-all;
  }

  .step-desc strong {
    color: var(--dark);
    font-weight: 700;
  }

  /* Apply Payment Info */
  .apply-payment-info {
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  .payment-title {
    font-size: 15px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 6px;
    position: relative;
    padding-left: 10px;
  }

  .payment-title::before {
    content: '';
    position: absolute;
    left: 0;
    top: 5px;
    bottom: 5px;
    width: 3px;
    background-color: var(--primary);
    border-radius: 1px;
  }

  .payment-text {
    font-size: 14px;
    color: var(--text);
    line-height: 1.6;
    word-break: keep-all;
  }

  /* CTA Button styling */
  .cta-button-container {
    margin-top: 50px;
  }

  .cta-apply-btn {
    display: inline-flex;
    align-items: center;
    gap: 12px;
    background-color: var(--primary);
    color: #ffffff;
    padding: 18px 45px;
    border: none;
    border-radius: 50px;
    font-size: 16px;
    font-weight: 700;
    cursor: pointer;
    box-shadow: 0 6px 20px rgba(255, 121, 0, 0.3);
    transition: all var(--transition-speed) ease;
  }

  .cta-apply-btn:hover {
    background-color: var(--primary-hover);
    transform: translateY(-3px);
    box-shadow: 0 8px 25px rgba(255, 121, 0, 0.45);
  }

  .cta-arrow {
    transition: transform var(--transition-speed) ease;
  }

  .cta-apply-btn:hover .cta-arrow {
    transform: translateX(6px);
  }

  /* Responsive Design adjustments */
  @media (max-width: 1200px) {
    .carousel-arrow.prev {
      left: 20px;
    }
    .carousel-arrow.next {
      right: 20px;
    }
  }

  @media (max-width: 992px) {
    .hero-header-row {
      flex-direction: column;
      align-items: flex-start;
      gap: 25px;
    }

    .hero-badge-area {
      align-self: flex-start;
    }

    .carousel-slide {
      height: 320px;
    }

    .details-grid {
      grid-template-columns: 1fr;
      max-width: 650px;
      margin: 0 auto;
    }

    .split-card {
      flex-direction: column;
    }

    .card-left-block {
      flex: 0 0 auto;
      flex-direction: row;
      width: 100%;
      padding: 15px 25px;
      justify-content: flex-start;
      gap: 12px;
    }

    .card-left-icon {
      width: 28px;
      height: 28px;
    }

    .card-left-title {
      font-size: 16px;
      text-align: left;
    }

    .card-right-block {
      padding: 25px;
    }

    .flex-row-layout {
      flex-direction: column;
      align-items: flex-start;
      gap: 15px;
    }

    .double-layout-card .card-right-block.flex-row {
      flex-direction: column;
    }

    .card-text-side {
      padding: 25px;
    }

    .card-img-side {
      width: 100%;
      height: 240px;
    }

    .apply-cards-grid {
      grid-template-columns: 1fr;
      max-width: 650px;
      margin: 0 auto;
    }
  }

  @media (max-width: 768px) {
    .hero-section {
      padding-top: 100px;
      padding-bottom: 30px;
    }

    .hero-title {
      font-size: 28px;
    }

    .hero-subtitle {
      font-size: 14px;
    }

    .circular-badge {
      width: 80px;
      height: 80px;
      font-size: 12px;
    }

    .carousel-slide {
      height: 240px;
    }

    .sub-nav-wrapper.sticky {
      top: 60px; /* Header shrinks on mobile */
    }

    .section-title {
      font-size: 24px;
      margin-bottom: 20px;
    }

    .intro-desc-card {
      padding: 25px;
    }

    .intro-desc-list li {
      font-size: 14px;
    }

    .apply-section {
      padding: 60px 0;
      background-attachment: scroll;
    }

    .apply-subheading {
      font-size: 16px;
      margin-bottom: 25px;
    }

    .cta-apply-btn {
      padding: 15px 35px;
      font-size: 15px;
      width: 100%;
      max-width: 280px;
      justify-content: center;
    }
  }
</style>
