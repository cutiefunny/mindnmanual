<script>
  import { onMount } from 'svelte';
  import Header from '$lib/components/Header.svelte';
  import Footer from '$lib/components/Footer.svelte';
  import FloatingActions from '$lib/components/FloatingActions.svelte';
  import Certifications from '$lib/components/Certifications.svelte';

  // Carousel slider state
  let currentSlide = $state(0);
  const slides = [
    'https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/edfe7f5f77ede.jpg',
    'https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/b6c9476144377.jpg',
    'https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/4b0d120065e3b.jpg',
    'https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/1be560d7527fd.jpg'
  ];

  // Sticky sub-navigation state
  let stickyMenu = $state(false);
  let activeSection = $state('intro');

  const navItems = [
    { id: 'intro', name: '프로그램 소개' },
    { id: 'apply', name: '교육 신청' }
  ];

  // Contact Modal state
  let modalOpen = $state(false);
  let formSubmitted = $state(false);
  let formData = $state({
    groupName: '',
    contactName: '',
    phone: '',
    email: '',
    date: '',
    headcount: '',
    message: ''
  });

  onMount(() => {
    // Background slide rotation
    const slideInterval = setInterval(() => {
      currentSlide = (currentSlide + 1) % slides.length;
    }, 4500);

    // Scroll handler for sticky sub-nav and section active highlighting
    const handleScroll = () => {
      const scrollY = window.scrollY;
      stickyMenu = scrollY > 400; // Triggers sticky position below hero section

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

  function openModal() {
    modalOpen = true;
    formSubmitted = false;
    // reset form
    formData = {
      groupName: '',
      contactName: '',
      phone: '',
      email: '',
      date: '',
      headcount: '',
      message: ''
    };
  }

  function closeModal() {
    modalOpen = false;
  }

  function handleSubmit(event) {
    event.preventDefault();
    // In a real application, this would send formData to a backend server.
    console.log('Submitted Inquiry Data:', formData);
    formSubmitted = true;
    setTimeout(() => {
      closeModal();
    }, 2500);
  }
</script>

<svelte:head>
  <title>스마트 심폐소생술 교육 | 마인드앤매뉴얼</title>
  <meta name="description" content="마인드앤매뉴얼의 시그니처 스마트 심폐소생술 교육 과정. IoT 교구재와 프로그램, 실시간 피드백 시스템을 통해 정확한 가슴압박과 실습 방식을 교육합니다." />
</svelte:head>

<Header />

<!-- Page Hero Section with Image Carousel Background -->
<section class="smart-hero">
  <div class="slides-container">
    {#each slides as slide, index}
      <div 
        class="slide {index === currentSlide ? 'active' : ''}" 
        style="background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.45) 0%, rgba(0, 0, 0, 0.65) 100%), url('{slide}')"
      ></div>
    {/each}
  </div>
  <div class="container hero-content animate-fade-in">
    <div class="hero-badges">
      <span class="hero-badge orange-badge">기업 / 단체</span>
      <span class="hero-badge border-badge">출강</span>
    </div>
    <h1 class="hero-title">스마트 심폐소생술 교육</h1>
    <div class="hero-divider"></div>
    <p class="hero-subtitle">스마트 심폐소생술 과정은 마인드앤매뉴얼의 시그니처 심폐소생술 교육입니다.</p>
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
    <div class="section-header text-center animate-slide-up">
      <span class="section-tag">Program Introduction</span>
      <h2 class="section-title">프로그램 소개</h2>
    </div>

    <!-- Intro description text boxes -->
    <div class="intro-desc-wrapper animate-slide-up">
      <div class="intro-desc-card">
        <p class="intro-desc-text">
          질병관리청 표준 자료를 중심으로 IoT교구재와 프로그램을 활용하여 교육생의 학습상태를 정확하게 모니터링하고 피드백 할 수 있습니다.
        </p>
        <p class="intro-desc-text">
          연령 별 심폐소생술을 체득할 수 있는 선진적인 교육과정으로 기업 및 단체 교육에 적합하며 출장 강의 형태로 진행됩니다.
        </p>
        <p class="intro-desc-text font-weight-500">
          스마트 심폐소생술 교육은 최근 개정 및 강화된 『중대재해 처벌 등에 관한 법률』, 『산업안전보건법』등의 기준을 근거로 하여 응급상황시 동료의 소중한 생명을 구할 수 있는 올바른 응급처치법을 배울 수 있도록 구성되어 있습니다.
        </p>
      </div>
    </div>

    <!-- Detailed 2x2 Grid -->
    <div class="details-grid animate-slide-up">
      
      <!-- Card 1: 대상 / 비용 -->
      <div class="details-card">
        <div class="card-header">
          <div class="header-left">
            <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/6f92696af91cb.png" alt="대상 비용 아이콘" class="card-icon" />
            <h3 class="card-title">대상 / 비용</h3>
          </div>
        </div>
        <div class="card-body">
          <ul class="body-list">
            <li><span class="bullet-label">대상 :</span> 기업/단체</li>
            <li><span class="bullet-label">교육 비용 :</span> 15,000~25,000원/인 (VAT 별도)</li>
            <li><span class="bullet-label">수료증 발급비용 :</span> 5,000원/인 (VAT 별도)</li>
            <li class="notice-item">* 교육 비용은 인원에 따라 표기된 금액 내 조정 가능</li>
          </ul>
        </div>
      </div>

      <!-- Card 2: 교육 내용 -->
      <div class="details-card">
        <div class="card-header">
          <div class="header-left">
            <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/b71f0441e2063.png" alt="교육 내용 아이콘" class="card-icon" />
            <h3 class="card-title">교육 내용</h3>
          </div>
          <span class="time-badge">교육 시간 : 120분</span>
        </div>
        <div class="card-body">
          <ul class="body-list check-list">
            <li>사고사례</li>
            <li>반응확인 및 신고절차</li>
            <li>성인/영아 가슴압박소생술</li>
            <li>자동심장충격기 사용법</li>
            <li>성인/영아 기도폐쇄 처치법</li>
            <li>평가</li>
          </ul>
        </div>
      </div>

      <!-- Card 3: 강사 구성 -->
      <div class="details-card">
        <div class="card-header">
          <div class="header-left">
            <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/1efbaf3bafab4.png" alt="강사 구성 아이콘" class="card-icon" />
            <h3 class="card-title">강사 구성</h3>
          </div>
        </div>
        <div class="card-body">
          <ul class="body-list">
            <li>의료인 및 대한심폐소생협회 강사</li>
            <li><span class="bullet-label">진행강사 :</span> 1명 (전반적인 강의 및 실습 진행)</li>
            <li><span class="bullet-label">보조강사 :</span> 실습보조 (교육생 인원에 따라 추가)</li>
          </ul>
        </div>
      </div>

      <!-- Card 4: 실습 교구 -->
      <div class="details-card double-layout-card">
        <div class="card-text-side">
          <div class="card-header no-border-padding">
            <div class="header-left">
              <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/6197b24ed1066.png" alt="실습 교구 아이콘" class="card-icon" />
              <h3 class="card-title">실습 교구</h3>
            </div>
          </div>
          <div class="card-body no-padding">
            <ul class="body-list check-list">
              <li>성인 마네킨</li>
              <li>영아 마네킨</li>
              <li>자동심장충격기(AED)</li>
              <li>기도폐쇄 조끼</li>
              <li>피드백 디바이스(큐브)</li>
            </ul>
          </div>
        </div>
        <div class="card-img-side">
          <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/b732fbdf87856.png" alt="실습 교구 모음" class="tools-img" />
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
      <span class="section-tag light">Application Information</span>
      <h2 class="section-title text-white">신청 방법 / 비용 납입</h2>
    </div>

    <div class="apply-cards-grid">
      <!-- Card Left: 신청 방법 -->
      <div class="apply-card">
        <h3 class="apply-card-title">I 신청 방법</h3>
        <ul class="apply-steps">
          <li>
            <span class="step-num">1</span>
            <p class="step-text">하단의 <strong>[교육신청하기]</strong> 버튼을 클릭합니다.</p>
          </li>
          <li>
            <span class="step-num">2</span>
            <p class="step-text">신청양식에 따라 상세하게 정보를 기재하여 신청을 접수합니다.</p>
          </li>
          <li>
            <span class="step-num">3</span>
            <p class="step-text">신청이 완료되면 마인드앤매뉴얼 담당자가 개별 연락을 통해 세부 교육과정 상담을 진행합니다.</p>
          </li>
          <li>
            <span class="step-num">4</span>
            <p class="step-text">일정 및 세부 조율이 필요한 경우 유선상담을 통해 변경사항을 상시 접수합니다.</p>
          </li>
        </ul>
      </div>

      <!-- Card Right: 비용 납입 -->
      <div class="apply-card">
        <h3 class="apply-card-title">I 비용 납입</h3>
        <ul class="apply-payment-info">
          <li>
            <svg class="payment-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <rect x="3" y="4" width="18" height="16" rx="2" ry="2"/>
              <line x1="3" y1="10" x2="21" y2="10"/>
            </svg>
            <p class="payment-text">교육 수료 후 30일 이내에 납부해 주시면 됩니다.</p>
          </li>
          <li>
            <svg class="payment-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
              <polyline points="14 2 14 8 20 8"/>
              <line x1="16" y1="13" x2="8" y2="13"/>
              <line x1="16" y1="17" x2="8" y2="17"/>
              <polyline points="10 9 9 9 8 9"/>
            </svg>
            <p class="payment-text">세금계산서 발행 및 카드 결제가 모두 가능합니다.</p>
          </li>
        </ul>
      </div>
    </div>

    <!-- CTA Trigger Button -->
    <div class="cta-button-container text-center">
      <button class="cta-apply-btn" onclick={openModal} id="open-apply-modal-btn">
        <span>교육신청하기</span>
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="cta-arrow">
          <line x1="5" y1="12" x2="19" y2="12"></line>
          <polyline points="12 5 19 12 12 19"></polyline>
        </svg>
      </button>
    </div>

  </div>
</section>

<!-- Interactive Modal for Education Inquiry -->
{#if modalOpen}
  <div class="modal-backdrop" onclick={closeModal}></div>
  <div class="modal-wrapper animate-zoom-in" role="dialog" aria-modal="true" aria-labelledby="modal-title">
    <div class="modal-header">
      <h2 id="modal-title" class="modal-header-title">스마트 심폐소생술 교육 신청 문의</h2>
      <button class="modal-close-btn" onclick={closeModal} aria-label="Close modal">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M18 6L6 18M6 6l12 12" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
    </div>
    
    <div class="modal-body">
      {#if formSubmitted}
        <div class="success-message text-center">
          <div class="success-icon-box">
            <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" class="success-icon">
              <polyline points="20 6 9 17 4 12"></polyline>
            </svg>
          </div>
          <h3>신청 접수 완료</h3>
          <p>교육 신청 문의가 성공적으로 접수되었습니다.<br/>담당자가 신속하게 검토한 후 입력해주신 연락처로 연락드리겠습니다.</p>
        </div>
      {:else}
        <form onsubmit={handleSubmit} class="inquiry-form">
          <div class="form-grid">
            <div class="form-group">
              <label for="groupName" class="form-label">기관 / 단체명 <span class="required">*</span></label>
              <input type="text" id="groupName" class="form-input" placeholder="예: (주)마인드컴퍼니" bind:value={formData.groupName} required />
            </div>
            
            <div class="form-group">
              <label for="contactName" class="form-label">담당자명 <span class="required">*</span></label>
              <input type="text" id="contactName" class="form-input" placeholder="예: 홍길동" bind:value={formData.contactName} required />
            </div>

            <div class="form-group">
              <label for="phone" class="form-label">연락처 <span class="required">*</span></label>
              <input type="tel" id="phone" class="form-input" placeholder="예: 010-1234-5678" bind:value={formData.phone} required />
            </div>

            <div class="form-group">
              <label for="email" class="form-label">이메일 주소 <span class="required">*</span></label>
              <input type="email" id="email" class="form-input" placeholder="예: contact@domain.com" bind:value={formData.email} required />
            </div>

            <div class="form-group">
              <label for="date" class="form-label">희망 교육 일정</label>
              <input type="date" id="date" class="form-input" bind:value={formData.date} />
            </div>

            <div class="form-group">
              <label for="headcount" class="form-label">예상 교육 인원 (명)</label>
              <input type="number" id="headcount" class="form-input" placeholder="예: 30" min="1" bind:value={formData.headcount} />
            </div>
          </div>

          <div class="form-group full-width">
            <label for="message" class="form-label">추가 문의사항</label>
            <textarea id="message" class="form-textarea" rows="4" placeholder="교육 장소, 특별 희망사항이나 추가 요청 사항 등을 자유롭게 적어주세요." bind:value={formData.message}></textarea>
          </div>

          <div class="form-actions text-center">
            <button type="button" class="form-btn cancel-btn" onclick={closeModal}>취소</button>
            <button type="submit" class="form-btn submit-btn">문의하기</button>
          </div>
        </form>
      {/if}
    </div>
  </div>
{/if}

<Certifications />
<Footer />
<FloatingActions />

<style>
  /* Base margins / scroll alignment */
  section {
    scroll-margin-top: 135px; /* Offset to clear header + sticky subnav */
  }

  /* Hero Section Styling */
  .smart-hero {
    position: relative;
    height: 540px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ffffff;
    text-align: center;
    overflow: hidden;
    padding-top: 90px; /* Header space offset */
  }

  .slides-container {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
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
    transition: opacity 1.5s ease-in-out;
  }

  .slide.active {
    opacity: 1;
  }

  .hero-content {
    position: relative;
    z-index: 2;
    max-width: 850px;
    padding: 0 20px;
  }

  .hero-badges {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-bottom: 20px;
  }

  .hero-badge {
    font-size: 13px;
    font-weight: 600;
    padding: 6px 16px;
    border-radius: 30px;
    text-transform: uppercase;
  }

  .orange-badge {
    background-color: var(--primary);
    color: #ffffff;
  }

  .border-badge {
    border: 1px solid rgba(255, 255, 255, 0.6);
    color: #ffffff;
  }

  .hero-title {
    font-size: 46px;
    font-weight: 700;
    margin-bottom: 15px;
    letter-spacing: -1.5px;
    line-height: 1.2;
    text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
  }

  .hero-divider {
    width: 60px;
    height: 3px;
    background-color: var(--primary);
    margin: 0 auto 22px;
  }

  .hero-subtitle {
    font-size: 18px;
    color: rgba(255, 255, 255, 0.9);
    font-weight: 400;
    text-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
    word-break: keep-all;
  }

  /* Sub Navigation Bar styling */
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
    top: 90px; /* Beneath header */
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
    overflow-x: auto;
    scrollbar-width: none;
  }

  .sub-nav-pills::-webkit-scrollbar {
    display: none;
  }

  .sub-nav-pill {
    padding: 10px 28px;
    background-color: var(--bg-white);
    border: 1px solid var(--border-color);
    border-radius: 30px;
    font-size: 14px;
    font-weight: 500;
    color: #555555;
    cursor: pointer;
    white-space: nowrap;
    transition: all var(--transition-speed);
  }

  .sub-nav-pill:hover, .sub-nav-pill.active {
    background-color: var(--primary);
    color: var(--bg-white);
    border-color: var(--primary);
    box-shadow: 0 4px 10px rgba(255, 121, 0, 0.15);
  }

  /* Section Header structure */
  .section-header {
    margin-bottom: 40px;
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

  .section-tag.light {
    color: #ff9d42;
  }

  .section-title {
    font-size: 32px;
    font-weight: 700;
    color: var(--dark);
    letter-spacing: -0.5px;
  }

  /* Section 1: Intro Section */
  .intro-section {
    background-color: var(--bg-white);
  }

  .intro-desc-wrapper {
    max-width: 960px;
    margin: 0 auto 50px;
  }

  .intro-desc-card {
    background-color: var(--bg-light);
    border: 1px solid var(--border-color);
    border-radius: 20px;
    padding: 35px 40px;
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .intro-desc-text {
    font-size: 16px;
    line-height: 1.8;
    color: var(--text);
    word-break: keep-all;
  }

  .font-weight-500 {
    font-weight: 500;
    color: var(--dark);
  }

  /* 2x2 Details Grid */
  .details-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 30px;
    max-width: 1200px;
    margin: 0 auto;
  }

  .details-card {
    background-color: var(--bg-white);
    border: 1px solid var(--border-color);
    border-radius: 20px;
    padding: 35px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.02);
    display: flex;
    flex-direction: column;
    height: 100%;
    transition: transform var(--transition-speed), box-shadow var(--transition-speed);
  }

  .details-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
  }

  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid var(--border-color);
    padding-bottom: 20px;
    margin-bottom: 24px;
    width: 100%;
  }

  .header-left {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .card-icon {
    width: 32px;
    height: 32px;
    object-fit: contain;
  }

  .card-title {
    font-size: 20px;
    font-weight: 700;
    color: var(--dark);
  }

  .time-badge {
    background-color: var(--primary);
    color: #ffffff;
    font-size: 12px;
    font-weight: 600;
    padding: 6px 16px;
    border-radius: 20px;
  }

  .card-body {
    flex-grow: 1;
  }

  .body-list {
    display: flex;
    flex-direction: column;
    gap: 14px;
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
    margin-right: 6px;
  }

  .notice-item {
    font-size: 13px !important;
    color: var(--text-muted) !important;
    margin-top: 5px;
  }

  /* Check list styling */
  .check-list li {
    position: relative;
    padding-left: 24px;
  }

  .check-list li::before {
    content: '';
    position: absolute;
    left: 4px;
    top: 6px;
    width: 10px;
    height: 6px;
    border-left: 2px solid var(--primary);
    border-bottom: 2px solid var(--primary);
    transform: rotate(-45deg);
  }

  /* Card 4 Double Layout Styling */
  .double-layout-card {
    flex-direction: row;
    gap: 30px;
    padding: 35px;
  }

  .card-text-side {
    flex: 1.2;
    display: flex;
    flex-direction: column;
  }

  .card-img-side {
    flex: 1;
    border-radius: 12px;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #f7f7f7;
    border: 1px solid var(--border-color);
  }

  .tools-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform var(--transition-speed);
  }

  .double-layout-card:hover .tools-img {
    transform: scale(1.05);
  }

  .no-border-padding {
    border: none;
    padding-bottom: 0;
    margin-bottom: 20px;
  }

  .no-padding {
    padding: 0;
  }

  /* Section 2: Education Application Styling */
  .apply-section {
    position: relative;
    width: 100%;
    padding: 100px 0;
    background-image: url('https://cdn.imweb.me/thumbnail/20230829/c17cb9de1ece3.png');
    background-size: cover;
    background-position: center;
    background-attachment: fixed;
  }

  .apply-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(22, 22, 22, 0.88);
    z-index: 1;
  }

  .apply-content {
    position: relative;
    z-index: 2;
  }

  .light-theme {
    margin-bottom: 50px;
  }

  .text-white {
    color: #ffffff !important;
  }

  .apply-cards-grid {
    display: grid;
    grid-template-columns: 1.2fr 1fr;
    gap: 30px;
    max-width: 1100px;
    margin: 0 auto 50px;
  }

  .apply-card {
    background-color: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 24px;
    padding: 40px;
    backdrop-filter: blur(12px);
  }

  .apply-card-title {
    font-size: 22px;
    font-weight: 700;
    color: #ffffff;
    margin-bottom: 30px;
  }

  .apply-steps {
    display: flex;
    flex-direction: column;
    gap: 24px;
  }

  .apply-steps li {
    display: flex;
    gap: 16px;
    align-items: flex-start;
  }

  .step-num {
    background-color: var(--primary);
    color: #ffffff;
    width: 26px;
    height: 26px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 13px;
    font-weight: 700;
    flex-shrink: 0;
    margin-top: 2px;
    box-shadow: 0 4px 10px rgba(255, 121, 0, 0.3);
  }

  .step-text {
    font-size: 15px;
    line-height: 1.7;
    color: rgba(255, 255, 255, 0.85);
    word-break: keep-all;
  }

  .step-text strong {
    color: #ffffff;
    font-weight: 600;
  }

  .apply-payment-info {
    display: flex;
    flex-direction: column;
    gap: 24px;
  }

  .apply-payment-info li {
    display: flex;
    gap: 16px;
    align-items: flex-start;
  }

  .payment-icon {
    color: var(--primary);
    flex-shrink: 0;
    margin-top: 2px;
  }

  .payment-text {
    font-size: 15px;
    line-height: 1.7;
    color: rgba(255, 255, 255, 0.85);
    word-break: keep-all;
  }

  /* CTA Apply Button */
  .cta-button-container {
    margin-top: 20px;
  }

  .cta-apply-btn {
    display: inline-flex;
    align-items: center;
    gap: 12px;
    background-color: var(--primary);
    color: var(--bg-white);
    padding: 16px 40px;
    border: none;
    border-radius: 50px;
    font-size: 16px;
    font-weight: 700;
    font-family: inherit;
    cursor: pointer;
    box-shadow: 0 6px 20px rgba(255, 121, 0, 0.3);
    transition: all var(--transition-speed);
  }

  .cta-apply-btn:hover {
    background-color: var(--primary-hover);
    transform: translateY(-3px);
    box-shadow: 0 8px 25px rgba(255, 121, 0, 0.4);
  }

  .cta-arrow {
    transition: transform var(--transition-speed) ease;
  }

  .cta-apply-btn:hover .cta-arrow {
    transform: translateX(5px);
  }

  /* Modal Dialog CSS */
  .modal-backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.6);
    backdrop-filter: blur(5px);
    z-index: 1001;
  }

  .modal-wrapper {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 90%;
    max-width: 680px;
    max-height: 90vh;
    background-color: var(--bg-white);
    border-radius: 24px;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15);
    z-index: 1002;
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  .modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 24px 30px;
    border-bottom: 1px solid var(--border-color);
  }

  .modal-header-title {
    font-size: 20px;
    font-weight: 700;
    color: var(--dark);
  }

  .modal-close-btn {
    background: none;
    border: none;
    color: var(--text-muted);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 4px;
    border-radius: 50%;
    transition: background-color 0.2s, color 0.2s;
  }

  .modal-close-btn:hover {
    background-color: var(--bg-light);
    color: var(--dark);
  }

  .modal-body {
    padding: 30px;
    overflow-y: auto;
    flex-grow: 1;
  }

  .inquiry-form {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .form-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
  }

  .form-group {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .full-width {
    grid-column: span 2;
  }

  .form-label {
    font-size: 14px;
    font-weight: 600;
    color: var(--dark);
  }

  .required {
    color: #ff3b30;
  }

  .form-input, .form-textarea {
    width: 100%;
    padding: 12px 16px;
    border: 1px solid #dcdcdc;
    border-radius: 10px;
    font-size: 14px;
    color: var(--text);
    background-color: var(--bg-white);
    transition: border-color 0.2s, box-shadow 0.2s;
  }

  .form-input:focus, .form-textarea:focus {
    outline: none;
    border-color: var(--primary);
    box-shadow: 0 0 0 3px rgba(255, 121, 0, 0.1);
  }

  .form-textarea {
    resize: none;
  }

  .form-actions {
    margin-top: 15px;
    display: flex;
    justify-content: center;
    gap: 12px;
  }

  .form-btn {
    padding: 12px 36px;
    border-radius: 30px;
    font-size: 15px;
    font-weight: 600;
    cursor: pointer;
    font-family: inherit;
    transition: all 0.2s;
  }

  .cancel-btn {
    background-color: var(--bg-light);
    color: var(--text-muted);
    border: 1px solid var(--border-color);
  }

  .cancel-btn:hover {
    background-color: #ededed;
    color: var(--dark);
  }

  .submit-btn {
    background-color: var(--primary);
    color: #ffffff;
    border: none;
    box-shadow: 0 4px 12px rgba(255, 121, 0, 0.2);
  }

  .submit-btn:hover {
    background-color: var(--primary-hover);
    transform: translateY(-1px);
    box-shadow: 0 6px 15px rgba(255, 121, 0, 0.3);
  }

  /* Success State Styling */
  .success-message {
    padding: 40px 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
  }

  .success-icon-box {
    width: 80px;
    height: 80px;
    border-radius: 50%;
    background-color: #e8f5e9;
    color: #2e7d32;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 6px 20px rgba(46, 125, 50, 0.15);
  }

  .success-message h3 {
    font-size: 22px;
    font-weight: 700;
    color: var(--dark);
  }

  .success-message p {
    font-size: 15px;
    line-height: 1.7;
    color: var(--text-muted);
    word-break: keep-all;
  }

  /* Responsive Design adjustments */
  @media (max-width: 992px) {
    .details-grid {
      grid-template-columns: 1fr;
      max-width: 600px;
    }

    .double-layout-card {
      flex-direction: column;
    }

    .card-img-side {
      height: 200px;
    }

    .apply-cards-grid {
      grid-template-columns: 1fr;
      max-width: 600px;
    }

    .form-grid {
      grid-template-columns: 1fr;
    }

    .full-width {
      grid-column: span 1;
    }
  }

  @media (max-width: 768px) {
    .smart-hero {
      height: 420px;
    }

    .hero-title {
      font-size: 32px;
    }

    .hero-subtitle {
      font-size: 15px;
    }

    .section-title {
      font-size: 26px;
    }

    .intro-desc-card {
      padding: 24px;
    }

    .intro-desc-text {
      font-size: 14px;
    }

    .details-card {
      padding: 24px;
    }

    .card-title {
      font-size: 18px;
    }

    .apply-section {
      background-attachment: scroll; /* Fallback for touch devices */
      padding: 60px 0;
    }

    .apply-card {
      padding: 24px;
    }

    .apply-card-title {
      font-size: 18px;
      margin-bottom: 20px;
    }

    .step-text, .payment-text {
      font-size: 14px;
    }

    .cta-apply-btn {
      padding: 14px 30px;
      font-size: 15px;
      width: 100%;
      max-width: 280px;
      justify-content: center;
    }

    .modal-header {
      padding: 16px 20px;
    }

    .modal-body {
      padding: 20px;
    }

    .modal-header-title {
      font-size: 18px;
    }
  }
</style>
