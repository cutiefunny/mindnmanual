<script>
  import { onMount } from 'svelte';

  let stickyMenu = $state(false);
  let activeSection = $state('qualifications');

  const navItems = [
    { id: 'qualifications', name: '대상 및 자격' },
    { id: 'timeline', name: '강사 양성과정 안내' }
  ];

  onMount(() => {
    const handleScroll = () => {
      const scrollY = window.scrollY;
      stickyMenu = scrollY > 260; // Triggers sticky subnav below the hero description

      // Active section detection
      for (const item of navItems) {
        const el = document.getElementById(item.id);
        if (el) {
          const rect = el.getBoundingClientRect();
          if (rect.top <= 160 && rect.bottom >= 160) {
            activeSection = item.id;
            break;
          }
        }
      }
    };

    window.addEventListener('scroll', handleScroll);
    handleScroll();

    return () => window.removeEventListener('scroll', handleScroll);
  });

  function scrollToSection(id) {
    const el = document.getElementById(id);
    if (el) {
      const offset = 140; // sticky header (70px) + sticky subnav (60px)
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
  <title>강사 지원 | 마인드앤매뉴얼</title>
  <meta name="description" content="마인드앤매뉴얼의 응급처치 강사 상시 모집 안내. 대상 및 자격, 주요 혜택 및 강사 양성과정 정보를 안내합니다." />
</svelte:head>



<!-- Page Hero Banner -->
<section class="apply-hero-banner">
  <div class="container hero-container animate-fade-in">
    <h1 class="hero-title">강사 지원</h1>
    <div class="hero-subtitle-divider"></div>
    <p class="hero-desc">
      마인드앤매뉴얼에서는 응급처치 교육을 적극적으로 전달해주실 의료인 강사를 상시 모집하고 있습니다.<br />
      강의나 교육 활동에 관심 있거나, 강사 자격은 있지만 강의 경험이 부족하신 분들,<br />
      현재 활동하고 있지만 강의를 좀 더 적극적으로 하고 싶은 분들 모두 지원하실 수 있습니다.
    </p>
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

<!-- Section 1: Qualifications & Benefits -->
<section id="qualifications" class="apply-section py-section">
  <div class="container section-container">
    <div class="qual-benefits-grid animate-slide-up">
      <!-- Target & Qualifications Box -->
      <div class="info-card-box">
        <div class="card-header-icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2M9 11a4 4 0 1 0 0-8 4 4 0 0 0 0 8zm14-1h-6m6 4h-6"/>
          </svg>
        </div>
        <h2 class="card-box-title">대상 및 자격</h2>
        <div class="card-divider"></div>
        <ul class="styled-list">
          <li>의사, 간호사, 응급구조사(1, 2급) 등 의료인 면허를 소지하신 분</li>
          <li>대한심폐소생협회 강사 자격을 취득하신 분</li>
        </ul>
        <h4 class="card-sub-title">주요 활동 내용</h4>
        <ul class="styled-list">
          <li>온/오프라인 응급처치 교육 진행</li>
          <li>공헌활동 및 지원 행사 참여</li>
          <li>교육 컨텐츠 개발</li>
        </ul>
      </div>

      <!-- Benefits Box -->
      <div class="info-card-box highlight-box">
        <div class="card-header-icon">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/>
          </svg>
        </div>
        <h2 class="card-box-title">주요 혜택</h2>
        <div class="card-divider orange-divider"></div>
        <ul class="styled-list">
          <li>다양한 강의 기회 제공 및 강사료 지급</li>
          <li>교육 진행 시 실습 교구 일체 지원</li>
          <li>정기적인 워크샵 및 강사 스킬 업 트레이닝 프로그램 운영</li>
          <li>마인드앤매뉴얼 내 타 서비스 참가 비용 전액 면제</li>
        </ul>
      </div>
    </div>
  </div>
</section>

<!-- Section 2: Training Course Track Cards (Booking Links) -->
<section class="tracks-section py-section bg-light">
  <div class="container section-container">
    <div class="section-header text-center">
      <span class="section-tag">Instructor Programs</span>
      <h2 class="section-title">교육 과정 신청 및 예약</h2>
      <p class="section-subtitle">원하는 양성과정 트랙을 선택하여 일정을 확인하고 예약하세요.</p>
    </div>

    <div class="tracks-grid animate-slide-up">
      <!-- Card 1: CPR Messengers (University Students) -->
      <a href="https://naver.me/x9zGe9Ga" target="_blank" rel="noopener noreferrer" class="track-card">
        <div class="track-img-box">
          <img src="https://cdn.imweb.me/thumbnail/20260402/49e4165991ce3.jpg" alt="CPR 메신저스" />
          <div class="track-img-overlay"></div>
        </div>
        <div class="track-info">
          <span class="track-tag">대학생 심폐소생술 강사 양성</span>
          <h3 class="track-title">CPR 메신저스</h3>
          <p class="track-desc">일정 확인 및 네이버 예약 바로가기 &rarr;</p>
        </div>
      </a>

      <!-- Card 2: Instructor Academy (General / Medical Professionals) -->
      <a href="https://naver.me/FmGpQFJo" target="_blank" rel="noopener noreferrer" class="track-card">
        <div class="track-img-box">
          <img src="https://cdn.imweb.me/thumbnail/20260402/6e3852674b3c2.jpg" alt="의료인 심폐소생술 강사 양성" />
          <div class="track-img-overlay"></div>
        </div>
        <div class="track-info">
          <span class="track-tag">의료인 심폐소생술 강사 양성</span>
          <h3 class="track-title">인스트럭터 아카데미</h3>
          <p class="track-desc">일정 확인 및 네이버 예약 바로가기 &rarr;</p>
        </div>
      </a>
    </div>
  </div>
</section>

<!-- Section 3: Training Course Steps (Timeline) -->
<section id="timeline" class="apply-section py-section">
  <div class="container section-container">
    <div class="section-header text-center">
      <span class="section-tag">Training Course</span>
      <h2 class="section-title">강사 양성과정 안내</h2>
      <p class="section-subtitle">마인드앤매뉴얼의 공식 강사로 활동하기 위한 육성 로드맵입니다.</p>
    </div>

    <!-- Timeline Steps -->
    <div class="timeline-container animate-slide-up">
      <div class="timeline-line"></div>
      
      <div class="timeline-step">
        <div class="step-num">01</div>
        <div class="step-content">
          <h3 class="step-title">서류 접수 및 인터뷰</h3>
          <p class="step-desc">지원 서류를 검토하고, 화상 혹은 오프라인 인터뷰를 통해 강사 적합성을 확인합니다.</p>
        </div>
      </div>

      <div class="timeline-step">
        <div class="step-num">02</div>
        <div class="step-content">
          <h3 class="step-title">심폐소생술 교육 표준 프로그램 이수</h3>
          <p class="step-desc">기본 지식 일치를 위한 가이드라인 및 심폐소생술 표준 교육을 이수합니다. (오프라인, 2~3시간)</p>
        </div>
      </div>

      <div class="timeline-step">
        <div class="step-num">03</div>
        <div class="step-content">
          <h3 class="step-title">강의 스킬 업 1:1 트레이닝</h3>
          <p class="step-desc">강의 시나리오 리허설, 프레젠테이션 스킬, 질의응답 피드백 훈련을 실시합니다. (오프라인, 8시간)</p>
        </div>
      </div>

      <div class="timeline-step">
        <div class="step-num">04</div>
        <div class="step-content">
          <h3 class="step-title">강사 자격증 교부</h3>
          <p class="step-desc">트레이닝 과정을 정상 이수한 분들을 대상으로 마인드앤매뉴얼 파트너 강사 자격증을 수여합니다.</p>
        </div>
      </div>

      <div class="timeline-step">
        <div class="step-num">05</div>
        <div class="step-content">
          <h3 class="step-title">교육 현장 실습 및 트레이닝</h3>
          <p class="step-desc">실제 공인 교육 현장에 보조 강사로 동행하여 참관 및 강의 실습을 진행합니다. (강사비 전액 지급)</p>
        </div>
      </div>
    </div>

    <!-- Course Fee Block -->
    <div class="fee-card-box animate-slide-up">
      <div class="fee-title-row">
        <svg class="fee-icon" width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="12" cy="12" r="10"/><line x1="12" y1="16" x2="12" y2="12"/><line x1="12" y1="8" x2="12.01" y2="8"/>
        </svg>
        <span>양성과정 참가비 안내</span>
      </div>
      <div class="fee-content-row">
        <div class="fee-item">
          <span class="fee-label">대학생 트랙</span>
          <span class="fee-price">20만원</span>
        </div>
        <div class="fee-divider-v"></div>
        <div class="fee-item">
          <span class="fee-label">의료인 및 일반 트랙</span>
          <span class="fee-price">36만원</span>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- Section 4: Call to Action Form Apply -->
<section class="apply-cta-section py-section bg-light">
  <div class="container text-center animate-slide-up">
    <h2 class="cta-title">지금 강사 양성과정에 지원하세요</h2>
    <p class="cta-desc">
      간단한 양식의 네이버 폼 지원서를 작성하여 신청해주시면,<br />
      담당자가 확인 후 서류 검토 및 인터뷰를 조율하기 위해 개별 연락을 드립니다.
    </p>
    <div class="cta-button-wrapper">
      <a 
        href="https://naver.me/FetGJ2Ql" 
        target="_blank" 
        rel="noopener noreferrer" 
        class="cta-apply-btn"
      >
        <svg class="btn-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="16" y1="13" x2="8" y2="13"/><line x1="16" y1="17" x2="8" y2="17"/><polyline points="10 9 9 9 8 9"/>
        </svg>
        <span>강사 지원서 작성하기 (교육 신청)</span>
      </a>
    </div>
  </div>
</section>



<style>
  /* Base settings */
  section {
    scroll-margin-top: 135px;
  }

  .apply-hero-banner {
    background: linear-gradient(135deg, #fefdfb 0%, #f6f3ed 100%);
    padding: 140px 0 90px;
    border-bottom: 1px solid var(--border-color);
  }

  .hero-container {
    text-align: center;
    max-width: 900px;
    margin: 0 auto;
  }

  .hero-title {
    font-size: 44px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 15px;
    letter-spacing: -1px;
  }

  .hero-subtitle-divider {
    width: 50px;
    height: 3px;
    background-color: var(--primary);
    margin: 0 auto 20px;
  }

  .hero-desc {
    font-size: 16px;
    line-height: 1.8;
    color: var(--text-muted);
    font-weight: 400;
    word-break: keep-all;
  }

  /* Sub Navigation */
  .sub-nav-wrapper {
    background-color: var(--bg-white);
    border-bottom: 1px solid var(--border-color);
    padding: 15px 0;
    z-index: 900;
    width: 100%;
    transition: all 0.3s ease;
  }

  .sub-nav-wrapper.sticky {
    position: fixed;
    top: 90px;
    left: 0;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.04);
    background-color: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
  }

  :global(.header.scrolled) ~ .sub-nav-wrapper.sticky {
    top: 70px;
  }

  .sub-nav-container {
    display: flex;
    justify-content: center;
  }

  .sub-nav-pills {
    display: flex;
    gap: 12px;
  }

  .sub-nav-pill {
    padding: 10px 24px;
    background-color: var(--bg-white);
    border: 1px solid var(--border-color);
    border-radius: 30px;
    font-size: 14px;
    font-weight: 500;
    color: #555555;
    cursor: pointer;
    transition: all var(--transition-speed);
  }

  .sub-nav-pill:hover, .sub-nav-pill.active {
    background-color: var(--primary);
    color: var(--bg-white);
    border-color: var(--primary);
    box-shadow: 0 4px 10px rgba(255, 121, 0, 0.15);
  }

  /* Section Headers */
  .section-header {
    margin-bottom: 50px;
  }

  .section-tag {
    font-size: 13px;
    font-weight: 600;
    color: var(--primary);
    text-transform: uppercase;
    letter-spacing: 2px;
    display: inline-block;
    margin-bottom: 10px;
  }

  .section-title {
    font-size: 32px;
    font-weight: 700;
    color: var(--dark);
    letter-spacing: -0.5px;
  }

  .section-subtitle {
    font-size: 15px;
    color: var(--text-muted);
    margin-top: 10px;
  }

  /* Qualifications & Benefits Grid */
  .qual-benefits-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
    max-width: 1100px;
    margin: 0 auto;
  }

  .info-card-box {
    background-color: var(--bg-white);
    border: 1px solid var(--border-color);
    border-radius: 20px;
    padding: 40px;
    box-shadow: 0 5px 25px rgba(0, 0, 0, 0.02);
    transition: transform var(--transition-speed);
  }

  .info-card-box:hover {
    transform: translateY(-4px);
  }

  .info-card-box.highlight-box {
    background-color: #fffaf5;
    border-color: #ffd8ba;
  }

  .card-header-icon {
    width: 50px;
    height: 50px;
    background-color: #f1f1f1;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--dark);
    margin-bottom: 24px;
  }

  .highlight-box .card-header-icon {
    background-color: #ffe8d6;
    color: var(--primary);
  }

  .card-box-title {
    font-size: 24px;
    font-weight: 700;
    color: var(--dark);
  }

  .card-sub-title {
    font-size: 16px;
    font-weight: 600;
    color: var(--dark);
    margin: 28px 0 12px;
  }

  .card-divider {
    width: 30px;
    height: 3px;
    background-color: var(--dark);
    margin: 15px 0 25px;
  }

  .card-divider.orange-divider {
    background-color: var(--primary);
  }

  .styled-list {
    display: flex;
    flex-direction: column;
    gap: 12px;
  }

  .styled-list li {
    font-size: 15px;
    line-height: 1.6;
    color: #444444;
    position: relative;
    padding-left: 18px;
    word-break: keep-all;
  }

  .styled-list li::before {
    content: "✓";
    color: var(--primary);
    position: absolute;
    left: 0;
    font-weight: bold;
  }

  /* Tracks Section Cards */
  .tracks-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
    max-width: 1100px;
    margin: 0 auto;
  }

  .track-card {
    background-color: var(--bg-white);
    border-radius: 20px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.04);
    display: flex;
    flex-direction: column;
    transition: all var(--transition-speed);
  }

  .track-card:hover {
    transform: translateY(-6px);
    box-shadow: 0 15px 40px rgba(0, 0, 0, 0.08);
  }

  .track-img-box {
    width: 100%;
    height: 280px;
    overflow: hidden;
    position: relative;
  }

  .track-img-box img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.5s ease;
  }

  .track-card:hover .track-img-box img {
    transform: scale(1.04);
  }

  .track-img-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(to bottom, rgba(0,0,0,0) 40%, rgba(0,0,0,0.5) 100%);
  }

  .track-info {
    padding: 30px;
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .track-tag {
    font-size: 13px;
    font-weight: 600;
    color: var(--primary);
  }

  .track-title {
    font-size: 22px;
    font-weight: 700;
    color: var(--dark);
  }

  .track-desc {
    font-size: 14px;
    font-weight: 500;
    color: var(--text-muted);
    margin-top: 5px;
    transition: color var(--transition-speed);
  }

  .track-card:hover .track-desc {
    color: var(--primary);
  }

  /* Timeline step list */
  .timeline-container {
    position: relative;
    max-width: 800px;
    margin: 40px auto 0;
    padding-left: 50px;
  }

  .timeline-line {
    position: absolute;
    left: 19px;
    top: 10px;
    width: 2px;
    height: calc(100% - 20px);
    background-color: var(--border-color);
  }

  .timeline-step {
    position: relative;
    padding-bottom: 40px;
  }

  .timeline-step:last-child {
    padding-bottom: 0;
  }

  .step-num {
    position: absolute;
    left: -50px;
    top: 0;
    width: 40px;
    height: 40px;
    background-color: var(--bg-white);
    border: 2px solid var(--border-color);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 14px;
    font-weight: 700;
    color: var(--text-muted);
    z-index: 2;
    transition: all var(--transition-speed);
  }

  .timeline-step:hover .step-num {
    border-color: var(--primary);
    color: var(--primary);
    background-color: #fffaf5;
  }

  .step-content {
    background-color: var(--bg-white);
    padding: 8px 0;
  }

  .step-title {
    font-size: 18px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 6px;
  }

  .step-desc {
    font-size: 14px;
    line-height: 1.6;
    color: var(--text-muted);
    word-break: keep-all;
  }

  /* Fee Info Box */
  .fee-card-box {
    max-width: 800px;
    margin: 40px auto 0;
    background-color: var(--bg-light);
    border-radius: 16px;
    padding: 30px;
    display: flex;
    flex-direction: column;
    gap: 20px;
    border: 1px dashed var(--border-color);
  }

  .fee-title-row {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 15px;
    font-weight: 600;
    color: var(--dark);
  }

  .fee-icon {
    color: var(--primary);
  }

  .fee-content-row {
    display: flex;
    justify-content: space-around;
    align-items: center;
  }

  .fee-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 6px;
  }

  .fee-label {
    font-size: 13px;
    color: var(--text-muted);
  }

  .fee-price {
    font-size: 20px;
    font-weight: 700;
    color: var(--dark);
  }

  .fee-divider-v {
    width: 1px;
    height: 40px;
    background-color: var(--border-color);
  }

  /* Apply CTA Section */
  .apply-cta-section {
    background-color: #fff9f4;
    border-top: 1px solid #ffeada;
  }

  .cta-title {
    font-size: 28px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 12px;
  }

  .cta-desc {
    font-size: 15px;
    line-height: 1.7;
    color: var(--text-muted);
    margin-bottom: 35px;
    word-break: keep-all;
  }

  .cta-button-wrapper {
    display: flex;
    justify-content: center;
  }

  .cta-apply-btn {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    background-color: var(--primary);
    color: var(--bg-white);
    padding: 16px 36px;
    border-radius: 35px;
    font-size: 16px;
    font-weight: 600;
    box-shadow: 0 5px 20px rgba(255, 121, 0, 0.25);
    transition: all var(--transition-speed);
  }

  .cta-apply-btn:hover {
    background-color: var(--primary-hover);
    transform: translateY(-2px);
    box-shadow: 0 7px 25px rgba(255, 121, 0, 0.35);
  }

  .btn-icon {
    flex-shrink: 0;
  }

  /* Responsive styling */
  @media (max-width: 992px) {
    .qual-benefits-grid {
      grid-template-columns: 1fr;
      gap: 20px;
    }

    .tracks-grid {
      grid-template-columns: 1fr;
      max-width: 500px;
    }

    .track-img-box {
      height: 220px;
    }
  }

  @media (max-width: 768px) {
    .hero-title {
      font-size: 32px;
    }

    .hero-desc {
      font-size: 14px;
    }

    .section-title {
      font-size: 26px;
    }

    .apply-hero-banner {
      padding: 120px 0 60px;
    }

    .info-card-box {
      padding: 30px;
    }

    .timeline-container {
      padding-left: 40px;
    }

    .step-num {
      left: -40px;
      width: 32px;
      height: 32px;
      font-size: 12px;
    }

    .fee-content-row {
      flex-direction: column;
      gap: 15px;
    }

    .fee-divider-v {
      display: none;
    }

    .cta-title {
      font-size: 22px;
    }
  }
</style>
