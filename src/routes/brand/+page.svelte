<script>
  import { onMount } from 'svelte';
  import Header from '$lib/components/Header.svelte';
  import Footer from '$lib/components/Footer.svelte';
  import FloatingActions from '$lib/components/FloatingActions.svelte';
  import Certifications from '$lib/components/Certifications.svelte';

  let stickyMenu = $state(false);
  let activeSection = $state('vision');

  const navItems = [
    { id: 'vision', name: '비전 & 목표' },
    { id: 'ci', name: 'CI 소개' },
    { id: 'features', name: '서비스 특징' },
    { id: 'certifications', name: '인증현황' }
  ];

  onMount(() => {
    const handleScroll = () => {
      // Sticky navigation detection
      const scrollY = window.scrollY;
      stickyMenu = scrollY > 260; // Triggers sticky position below the page title banner

      // Active section detection based on visibility threshold
      for (const item of navItems) {
        const el = document.getElementById(item.id);
        if (el) {
          const rect = el.getBoundingClientRect();
          // Active if section top is near the top of viewport (offset by header + sticky menu height)
          if (rect.top <= 160 && rect.bottom >= 160) {
            activeSection = item.id;
            break;
          }
        }
      }
    };

    window.addEventListener('scroll', handleScroll);
    handleScroll(); // Run initially to set correct states

    return () => window.removeEventListener('scroll', handleScroll);
  });

  function scrollToSection(id) {
    const el = document.getElementById(id);
    if (el) {
      const offset = 140; // sticky header (70px) + sticky subnav (60px) + padding safety
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
  <title>브랜드 소개 | 마인드앤매뉴얼</title>
  <meta name="description" content="마인드앤매뉴얼의 브랜드 비전, CI 및 응급처치 교육 서비스의 특징과 공인 인증 현황을 소개합니다." />
</svelte:head>

<Header />

<!-- Page Banner -->
<section class="brand-hero-banner">
  <div class="container hero-container animate-fade-in">
    <h1 class="hero-title">브랜드 소개</h1>
    <div class="hero-subtitle-divider"></div>
    <p class="hero-desc">We Learn Miracles - 생명을 살리는 기적을 배웁니다.</p>
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

<!-- Section 1: Vision & Goals -->
<section id="vision" class="brand-section py-section">
  <div class="container section-container">
    <div class="section-header text-center">
      <span class="section-tag">Vision & Goals</span>
      <h2 class="section-title">비전 & 목표</h2>
    </div>

    <div class="vision-content animate-slide-up">
      <div class="vision-image-wrapper">
        <img 
          src="https://cdn.imweb.me/thumbnail/20230824/a77f0e02df192.png" 
          alt="비전 & 목표 그래픽" 
          class="vision-image" 
        />
      </div>
      <div class="vision-text-wrapper">
        <p class="vision-paragraph">
          마인드앤매뉴얼은 더 많은 사람들이 응급처치술을 할 수 있도록 기여하겠다는 비전 아래 모든 국민을 대상으로 
          신속하고 효과적인 처치술의 중요성에 대한 공감대 형성과 학습 시 확실한 체득을 목표로 고품질의 
          응급처치 교육서비스를 전개하고 있습니다.
        </p>
      </div>
    </div>
  </div>
</section>

<div class="section-divider-line"></div>

<!-- Section 2: CI Introduction -->
<section id="ci" class="brand-section py-section bg-light">
  <div class="container section-container">
    <div class="section-header text-center">
      <span class="section-tag">Corporate Identity</span>
      <h2 class="section-title">CI 소개</h2>
    </div>

    <div class="ci-content-grid animate-slide-up">
      <!-- Left Column: Details 1 & 2 -->
      <div class="ci-detail-column left-col">
        <div class="ci-detail-card">
          <h3 class="ci-card-title">➊ 기업명 마인드앤매뉴얼</h3>
          <p class="ci-card-desc">
            응급처치교육에 대한 인식 개선 및 교육 확대와 과학적∙체계적인 트레이닝을 통한 기술 체득을 목표로 함
          </p>
        </div>
        
        <div class="ci-detail-card">
          <h3 class="ci-card-title">➋ 캐릭터 앤</h3>
          <p class="ci-card-desc">
            심폐소생술 마네킨의 얼굴과 응급처치술을 상징하는 적십자를 형상화
          </p>
        </div>
      </div>

      <!-- Center Column: Responsive Logo Graphic -->
      <div class="ci-logo-column">
        <div class="logo-image-box">
          <img 
            src="https://cdn.imweb.me/thumbnail/20260127/64be1f03d0a71.png" 
            alt="마인드앤매뉴얼 CI" 
            class="ci-logo-img ci-desktop" 
          />
          <img 
            src="https://cdn.imweb.me/thumbnail/20260127/342d6be210066.png" 
            alt="마인드앤매뉴얼 CI" 
            class="ci-logo-img ci-mobile" 
          />
        </div>
      </div>

      <!-- Right Column: Detail 3 & Download Button -->
      <div class="ci-detail-column right-col">
        <div class="ci-detail-card">
          <h3 class="ci-card-title">➌ 슬로건 we learn miracles</h3>
          <p class="ci-card-desc">
            누군가를 돕겠다는 적극적인 마음(mind)과 정확한 매뉴얼에 따른 행동(manual)은 소중한 생명을 살릴 수 있는 기적을 만들 수 있다는 의미
          </p>
        </div>

        <div class="ci-download-wrapper">
          <button type="button" class="ci-download-btn" onclick={() => alert('CI 다운로드가 준비 중입니다.')}>
            <svg class="download-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
              <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4M7 10l5 5 5-5M12 15V3"/>
            </svg>
            <span>CI 다운로드</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- Section 3: Service Features -->
<section id="features" class="features-parallax-section">
  <div class="features-overlay"></div>
  <div class="container features-content animate-slide-up">
    <div class="section-header text-center light-theme">
      <span class="section-tag light">Service Features</span>
      <h2 class="section-title text-white">서비스 특징</h2>
      <p class="features-intro-text">
        마인드앤매뉴얼은 제공하는 교육서비스의 신뢰도 확보를 위하여 교육 프로그램 보유, 전문인력 및 체험시설, 관련 교구 확보 등의 
        기관 운영 전반에 관한 검증을 통해 행정안전부 및 대한심폐소생협회 지정 응급처치분야 전문기관으로 인증을 받았습니다.
      </p>
    </div>

    <div class="features-grid">
      <!-- Feature Card 1 -->
      <div class="feature-card">
        <div class="feature-img-wrapper">
          <img src="https://cdn.imweb.me/thumbnail/20230821/9494977d4c79f.png" alt="프로그램" />
        </div>
        <div class="feature-text">
          <h3 class="feature-card-title">I 프로그램</h3>
          <ul class="feature-bullets">
            <li>질병관리청 한국 심폐소생술 프로그램을 중심으로 교육 내용이 표준화되어 있습니다.</li>
            <li>대한심폐소생협회의 가이드라인에 따라 학습 효과가 높은 국내∙외 선진적인 교육방식을 적용하였으며, 교육자료 및 실습방식을 주기적으로 업데이트 합니다.</li>
          </ul>
        </div>
      </div>

      <!-- Feature Card 2 -->
      <div class="feature-card">
        <div class="feature-img-wrapper">
          <img src="https://cdn.imweb.me/thumbnail/20230821/11cdce8007606.png" alt="강사" />
        </div>
        <div class="feature-text">
          <h3 class="feature-card-title">I 강사</h3>
          <ul class="feature-bullets">
            <li>강사진은 응급처치교육에 대한 전문적인 역량을 갖춘 의료인 및 대한심폐소생협회 강사로 구성되어 있습니다.</li>
            <li>강사의 강의 스킬 향상을 위한 다양한 부문의 전문 트레이닝을 실시하고, 교육내용 개선을 위한 정기적인 워크샵을 진행합니다.</li>
          </ul>
        </div>
      </div>

      <!-- Feature Card 3 -->
      <div class="feature-card">
        <div class="feature-img-wrapper">
          <img src="https://cdn.imweb.me/thumbnail/20230821/b55dc96b79697.png" alt="실습교구" />
        </div>
        <div class="feature-text">
          <h3 class="feature-card-title">I 실습교구</h3>
          <ul class="feature-bullets">
            <li>교육 진행 시 체득을 위한 CPR마네킨 및 AED트레이너 등 기본 실습 교구재가 교육생 인원에 맞춰 적정하게 설치됩니다.</li>
            <li>IoT 기반의 모니터링&피드백 디바이스 및 프로그램을 통해 학습 효과를 더욱 향상시키고, 실습 과정을 즐겁게 경험하실 수 있습니다.</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- Section 4: Certifications -->
<section id="certifications" class="brand-section py-section">
  <div class="container section-container">
    <div class="section-header text-center">
      <span class="section-tag">Accreditations</span>
      <h2 class="section-title">인증현황</h2>
    </div>
    <Certifications />
  </div>
</section>

<Footer />
<FloatingActions />

<style>
  /* Base margins / scroll alignment */
  section {
    scroll-margin-top: 135px; /* Offset to clear header + sticky subnav */
  }

  .brand-hero-banner {
    background: linear-gradient(135deg, #fefdfb 0%, #f6f3ed 100%);
    padding: 140px 0 90px;
    border-bottom: 1px solid var(--border-color);
  }

  .hero-container {
    text-align: center;
    max-width: 800px;
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
    font-size: 17px;
    color: var(--text-muted);
    font-weight: 400;
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
    top: 90px; /* Beneath uncompressed header */
    left: 0;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.04);
    background-color: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
  }

  /* Adjust for header compression on scroll */
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
    max-width: 100%;
    scrollbar-width: none;
  }

  .sub-nav-pills::-webkit-scrollbar {
    display: none;
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

  .section-tag.light {
    color: #ff9d42;
  }

  .section-title {
    font-size: 32px;
    font-weight: 700;
    color: var(--dark);
    letter-spacing: -0.5px;
  }

  /* Section Divider Line */
  .section-divider-line {
    width: 100%;
    height: 1px;
    background-color: var(--border-color);
  }

  /* Section 1: Vision Content */
  .vision-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 35px;
    max-width: 960px;
    margin: 0 auto;
  }

  .vision-image-wrapper {
    width: 100%;
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
  }

  .vision-image {
    width: 100%;
    height: auto;
    object-fit: cover;
  }

  .vision-text-wrapper {
    text-align: center;
    max-width: 800px;
  }

  .vision-paragraph {
    font-size: 18px;
    line-height: 1.8;
    color: #444444;
    word-break: keep-all;
    font-weight: 400;
  }

  /* Section 2: CI Content Grid */
  .ci-content-grid {
    display: grid;
    grid-template-columns: 1fr 1.2fr 1fr;
    gap: 40px;
    align-items: center;
    max-width: 1200px;
    margin: 0 auto;
  }

  .ci-detail-column {
    display: flex;
    flex-direction: column;
    gap: 30px;
  }

  .ci-detail-card {
    background-color: var(--bg-white);
    padding: 28px;
    border-radius: 16px;
    border: 1px solid rgba(0, 0, 0, 0.03);
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.02);
    transition: transform var(--transition-speed);
  }

  .ci-detail-card:hover {
    transform: translateY(-3px);
  }

  .ci-card-title {
    font-size: 18px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 12px;
  }

  .ci-card-desc {
    font-size: 14px;
    line-height: 1.6;
    color: var(--text-muted);
    word-break: keep-all;
  }

  .ci-logo-column {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .logo-image-box {
    width: 100%;
    display: flex;
    justify-content: center;
  }

  .ci-logo-img {
    max-width: 100%;
    height: auto;
    object-fit: contain;
  }

  .ci-mobile {
    display: none;
  }

  .ci-download-wrapper {
    margin-top: 10px;
    display: flex;
    justify-content: flex-start;
  }

  .ci-download-btn {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    background-color: var(--primary);
    color: var(--bg-white);
    padding: 14px 28px;
    border: none;
    border-radius: 30px;
    font-size: 15px;
    font-weight: 600;
    font-family: inherit;
    cursor: pointer;
    box-shadow: 0 5px 15px rgba(255, 121, 0, 0.2);
    transition: all var(--transition-speed);
  }

  .ci-download-btn:hover {
    background-color: var(--primary-hover);
    transform: translateY(-2px);
    box-shadow: 0 7px 20px rgba(255, 121, 0, 0.3);
  }

  .download-icon {
    flex-shrink: 0;
  }

  /* Section 3: Service Features with Parallax and Cards */
  .features-parallax-section {
    position: relative;
    width: 100%;
    padding: 100px 0;
    background-image: url('https://cdn.imweb.me/thumbnail/20230809/6dc1cebf3b4f9.png');
    background-size: cover;
    background-position: center;
    background-attachment: fixed;
    display: flex;
    align-items: center;
  }

  .features-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(15, 15, 15, 0.85);
    z-index: 1;
  }

  .features-content {
    position: relative;
    z-index: 2;
    width: 100%;
  }

  .features-intro-text {
    max-width: 900px;
    margin: 20px auto 0;
    font-size: 16px;
    line-height: 1.7;
    color: rgba(255, 255, 255, 0.75);
    word-break: keep-all;
  }

  .features-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 24px;
    margin-top: 50px;
  }

  .feature-card {
    background-color: rgba(255, 255, 255, 0.06);
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 20px;
    overflow: hidden;
    backdrop-filter: blur(10px);
    transition: all var(--transition-speed);
  }

  .feature-card:hover {
    transform: translateY(-5px);
    background-color: rgba(255, 255, 255, 0.1);
    border-color: rgba(255, 121, 0, 0.3);
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  }

  .feature-img-wrapper {
    width: 100%;
    height: 220px;
    overflow: hidden;
  }

  .feature-img-wrapper img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.5s ease;
  }

  .feature-card:hover .feature-img-wrapper img {
    transform: scale(1.05);
  }

  .feature-text {
    padding: 30px 24px;
  }

  .feature-card-title {
    font-size: 20px;
    font-weight: 700;
    color: #ffffff;
    margin-bottom: 20px;
  }

  .feature-bullets {
    display: flex;
    flex-direction: column;
    gap: 12px;
    padding-left: 0;
  }

  .feature-bullets li {
    font-size: 14px;
    line-height: 1.6;
    color: rgba(255, 255, 255, 0.75);
    position: relative;
    padding-left: 15px;
    word-break: keep-all;
  }

  .feature-bullets li::before {
    content: "•";
    color: var(--primary);
    font-size: 18px;
    position: absolute;
    left: 0;
    top: -2px;
  }

  /* Responsive Design Adjustments */
  @media (max-width: 992px) {
    .ci-content-grid {
      grid-template-columns: 1fr;
      gap: 30px;
    }

    .ci-detail-column.left-col {
      order: 1;
    }

    .ci-logo-column {
      order: 2;
      max-width: 400px;
      margin: 0 auto;
    }

    .ci-detail-column.right-col {
      order: 3;
    }

    .ci-download-wrapper {
      justify-content: center;
    }

    .ci-desktop {
      display: none;
    }

    .ci-mobile {
      display: block;
    }

    .features-grid {
      grid-template-columns: 1fr;
      gap: 30px;
      max-width: 600px;
      margin-left: auto;
      margin-right: auto;
    }

    .features-parallax-section {
      background-attachment: scroll; /* Fallback for touch screens */
      padding: 70px 0;
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

    .vision-paragraph {
      font-size: 15px;
    }

    .brand-hero-banner {
      padding: 120px 0 60px;
    }
  }
</style>
