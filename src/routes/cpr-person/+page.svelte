<script>
  import { onMount } from 'svelte';
  import Certifications from '$lib/components/Certifications.svelte';

  // Carousel slider state
  let currentSlide = $state(0);
  const slides = [
    { url: 'https://cdn.imweb.me/thumbnail/20230821/16c0d0a776be6.jpg', alt: '대한심폐소생협회 일반인 심폐소생술 교육 실습 1' },
    { url: 'https://cdn.imweb.me/thumbnail/20230821/e5f965e835205.jpg', alt: '대한심폐소생협회 일반인 심폐소생술 교육 실습 2' },
    { url: 'https://cdn.imweb.me/thumbnail/20230821/2b968e08cab12.jpg', alt: '대한심폐소생협회 일반인 심폐소생술 교육 실습 3' },
    { url: 'https://cdn.imweb.me/thumbnail/20230821/6257e97fb4c12.jpg', alt: '대한심폐소생협회 일반인 심폐소생술 교육 실습 4' }
  ];

  // Carousel layout and responsive width tracking
  let containerWidth = $state(0);
  let slideWidth = $derived(Math.min(800, containerWidth * 0.7));
  let gap = 24;
  let translateX = $derived((containerWidth / 2) - (currentSlide * (slideWidth + gap)) - (slideWidth / 2));

  // Classroom Carousel state
  let currentClassroomSlide = $state(0);
  const classroomSlides = [
    { url: 'https://cdn.imweb.me/thumbnail/20230814/366074ab24b97.png', alt: '트레이닝 센터 실습실 전경 1' },
    { url: 'https://cdn.imweb.me/thumbnail/20230814/6f63f82222f6e.png', alt: '트레이닝 센터 실습실 전경 2' },
    { url: 'https://cdn.imweb.me/thumbnail/20230814/49e6606d78601.png', alt: '트레이닝 센터 마네킨 및 교구 전경' },
    { url: 'https://cdn.imweb.me/thumbnail/20230814/9af84bfee06f5.png', alt: '트레이닝 센터 합정 로비 및 상담실' }
  ];

  // Sticky sub-navigation state
  let stickyMenu = $state(false);
  let activeSection = $state('intro');

  const navItems = [
    { id: 'intro', name: '프로그램 소개' },
    { id: 'basic', name: '기초과정' },
    { id: 'advanced', name: '심화과정' },
    { id: 'apply', name: '교육 신청' },
    { id: 'center', name: '트레이닝 센터 소개' }
  ];

  let slideInterval;
  let classroomInterval;

  onMount(() => {
    // Background slide rotation
    slideInterval = setInterval(() => {
      currentSlide = (currentSlide + 1) % slides.length;
    }, 4500);

    // Classroom slide rotation
    classroomInterval = setInterval(() => {
      currentClassroomSlide = (currentClassroomSlide + 1) % classroomSlides.length;
    }, 4000);

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

    // Naver Map Open API rendering
    const initNaverMap = () => {
      const loadMap = () => {
        const mapContainer = document.getElementById('naverMapContainer');
        if (mapContainer && window.naver && window.naver.maps) {
          mapContainer.innerHTML = '';
          const position = new window.naver.maps.LatLng(37.547623, 126.9173947);
          const mapOptions = {
            center: position,
            zoom: 16,
            zoomControl: true,
            zoomControlOptions: {
              position: window.naver.maps.Position.TOP_RIGHT
            }
          };
          const map = new window.naver.maps.Map(mapContainer, mapOptions);

          const marker = new window.naver.maps.Marker({
            position: position,
            map: map,
            title: "마인드앤매뉴얼 트레이닝 센터(합정)"
          });

          const infowindow = new window.naver.maps.InfoWindow({
            content: `
              <div style="padding: 10px; min-width: 180px; font-family: sans-serif; font-size: 12px; line-height: 1.5; color: #333;">
                <h4 style="margin: 0 0 5px 0; font-size: 13px; font-weight: 700; color: #ff7900;">마인드앤매뉴얼</h4>
                <p style="margin: 0;">서울시 마포구 독막로6길 8, 2층</p>
              </div>
            `,
            borderWidth: 1,
            borderColor: "#dddddd",
            anchorSize: new window.naver.maps.Size(10, 10),
            anchorSkew: true,
            anchorColor: "#ffffff"
          });

          infowindow.open(map, marker);
        }
      };

      if (window.naver && window.naver.maps) {
        loadMap();
      } else {
        const script = document.createElement('script');
        script.src = 'https://oapi.map.naver.com/openapi/v3/maps.js?ncpClientId=axrgdrlsoj';
        script.onload = loadMap;
        document.head.appendChild(script);
      }
    };

    initNaverMap();

    return () => {
      clearInterval(slideInterval);
      clearInterval(classroomInterval);
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

  function selectClassroomSlide(index) {
    clearInterval(classroomInterval);
    currentClassroomSlide = index;
    classroomInterval = setInterval(() => {
      currentClassroomSlide = (currentClassroomSlide + 1) % classroomSlides.length;
    }, 4000);
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
  <title>일반인 심폐소생술 교육(개인) | 마인드앤매뉴얼</title>
  <meta name="description" content="대한심폐소생협회 일반인 심폐소생술 교육과정. 개인 신청용 상설 교육과정으로 전문 강사진과 IoT 교구를 통한 체계적 실습을 제공합니다." />
</svelte:head>



<!-- Page Hero Section -->
<section class="hero-section">
  <div class="container hero-container-header animate-fade-in">
    <div class="hero-header-row">
      <div class="hero-title-area">
        <h1 class="hero-title">대한심폐소생협회<br />일반인 심폐소생술 교육</h1>
        <p class="hero-subtitle">해당 프로그램은 대한심폐소생협회에서 개발하고 인증하는 심폐소생술 개인 교육과정입니다.</p>
      </div>
      <div class="hero-badge-area">
        <div class="circular-badge">개인</div>
        <div class="circular-badge">트레이닝<br />센터</div>
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

    <!-- Intro description text boxes -->
    <div class="intro-desc-wrapper animate-slide-up">
      <div class="intro-desc-card">
        <ul class="intro-desc-list">
          <li>
            <span class="bullet-dot"></span>
            일반인 심폐소생술 기초/심화과정으로 구성되어 있으며, 기초과정은 성인 심폐소생술, 심화과정은 연령 별 심폐소생술과 기도 폐쇄 시 처치법을 학습할 수 있습니다.
          </li>
          <li>
            <span class="bullet-dot"></span>
            교육은 마인드앤매뉴얼 트레이닝센터에서 진행됩니다.
          </li>
          <li class="font-weight-500">
            <span class="bullet-dot"></span>
            대한심폐소생협회 일반인 심폐소생술 교육은 최근 개정 및 강화된 『중대재해 처벌 등에 관한 법률』, 『산업안전보건법』등의 기준을 근거로 하여 응급상황시 동료의 소중한 생명을 구할 수 있는 올바른 응급처치법을 배울 수 있도록 구성되어 있습니다.
          </li>
        </ul>
      </div>
    </div>
  </div>
</section>

<!-- Section 2: 기초과정 -->
<section id="basic" class="course-section py-section bg-light">
  <div class="container section-container">
    <div class="section-header animate-slide-up">
      <h2 class="section-title">기초과정</h2>
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
            <li><span class="bullet-label">대상 :</span> 개인</li>
            <li><span class="bullet-label">교육 비용 :</span> 20,000원/인</li>
            <li><span class="bullet-label">수료증 발급비용 :</span> 10,000원/인</li>
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
            <li>성인 가슴압박소생술</li>
            <li>자동심장충격기 사용법</li>
          </ul>
          <div class="time-badge">교육 시간 : 80분</div>
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
            <li><span class="bullet-label">진행강사 :</span> 1명 (전반적인 강의 및 실습 진행)</li>
            <li><span class="bullet-label">보조강사 :</span> 실습보조 (교육생 30명 초과시 추가)</li>
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
              <li>자동심장충격기(AED)</li>
            </ul>
          </div>
          <div class="card-img-side">
            <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/89c80c59635e1.png" alt="기초 실습 교구" />
          </div>
        </div>
      </div>

    </div>
  </div>
</section>

<!-- Section 3: 심화과정 -->
<section id="advanced" class="course-section py-section">
  <div class="container section-container">
    <div class="section-header animate-slide-up">
      <h2 class="section-title">심화과정</h2>
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
            <li><span class="bullet-label">대상 :</span> 개인</li>
            <li><span class="bullet-label">교육 비용 :</span> 30,000원/인</li>
            <li><span class="bullet-label">수료증 발급비용 :</span> 10,000원/인</li>
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
            <li>반응확인 및 신고절차</li>
            <li>연령 별 가슴압박소생술</li>
            <li>자동심장충격기 사용법</li>
            <li>성인/영아 기도폐쇄 처치법</li>
            <li>평가</li>
          </ul>
          <div class="time-badge">교육 시간 : 180분</div>
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
            <li><span class="bullet-label">진행강사 :</span> 1명 (전반적인 강의 및 실습 진행)</li>
            <li><span class="bullet-label">보조강사 :</span> 실습보조 (교육생 10명 초과시 추가)</li>
          </ul>
        </div>
      </div>

      <!-- Card 4: 실습 교구 (Double Layout) -->
      <div class="split-card double-layout-card">
        <div class="card-left-block">
          <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/5078a7408187b.png" alt="" class="card-left-icon" />
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
            <img src="https://cdn.imweb.me/upload/S20230801dc74a8f9b2b46/11630ad4a98f2.png" alt="심화 실습 교구" />
          </div>
        </div>
      </div>

    </div>
  </div>
</section>

<!-- Section 4: 교육 신청 -->
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
              <span class="step-desc">대한심폐소생협회 회원가입 (<a href="https://www.kacpr.org/" target="_blank" rel="noopener noreferrer">http://www.kacpr.org/</a>)</span>
            </li>
            <li>
              <span class="step-badge">2</span>
              <span class="step-desc"><strong>[교육안내] – [교육신청]</strong> 클릭</span>
            </li>
            <li>
              <span class="step-badge">3</span>
              <span class="step-desc">지역 <strong>‘서울’</strong>, 보건의료인/일반인 중 <strong>‘일반인’</strong> 선택 후 검색</span>
            </li>
            <li>
              <span class="step-badge">4</span>
              <span class="step-desc">원하는 날짜와 교육(기초/심화과정 구분)에 <strong>‘(주)마인드앤매뉴얼’</strong> 클릭</span>
            </li>
            <li>
              <span class="step-badge">5</span>
              <span class="step-desc">공고문 하단, <strong>‘교육신청하기’</strong> 클릭</span>
            </li>
            <li>
              <span class="step-badge">6</span>
              <span class="step-desc">교육생 선정 문자 또는 메일 확인</span>
            </li>
            <li>
              <span class="step-badge">7</span>
              <span class="step-desc">교육비 입금 후 신청한 교육일자에 참여</span>
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
              <h4 class="payment-title">교육 비용</h4>
              <p class="payment-text">- 마인드앤매뉴얼 사전 납부</p>
              <p class="payment-text">- 교육 신청 완료 시 납입 방식 안내</p>
            </li>
            <li class="payment-divider"></li>
            <li>
              <h4 class="payment-title">수료증 발급 비용</h4>
              <p class="payment-text">- 교육 수료 후 대한심폐소생협회 신청/납부</p>
              <p class="payment-text">- 수료증 발급 기간 3~4일 소요</p>
            </li>
            <li class="payment-divider"></li>
            <li>
              <h4 class="payment-title">환불규정</h4>
              <p class="payment-text">- 교육인원 미달 등으로 인한 폐강 시 : 100% 환불</p>
              <p class="payment-text">- 교육생 단순 변심 등으로 인한 취소 시 : 90% 환불</p>
              <p class="payment-text">- 당일 교육 취소 시 : 환불 불가</p>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <!-- CTA Trigger Button (External link to KACPR) -->
    <div class="cta-button-container text-center">
      <a class="cta-apply-btn" href="https://www.kacpr.org/" target="_blank" rel="noopener noreferrer">
        <span>대한심폐소생협회 교육신청</span>
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="cta-arrow">
          <line x1="5" y1="12" x2="19" y2="12"></line>
          <polyline points="12 5 19 12 12 19"></polyline>
        </svg>
      </a>
    </div>

  </div>
</section>

<!-- Section 5: 트레이닝 센터 소개 -->
<section id="center" class="center-section py-section bg-light">
  <div class="container section-container">
    <div class="section-header animate-slide-up">
      <h2 class="section-title">트레이닝 센터 소개</h2>
    </div>

    <div class="center-grid animate-slide-up">
      <!-- Left side: Map and Address -->
      <div class="map-wrapper">
        <div id="naverMapContainer" class="map-container"></div>
        <div class="address-bar-card">
          <div class="address-text">
            <strong>마인드앤매뉴얼 트레이닝 센터(합정)</strong>
            <p>서울시 독막로6길 8, 2층</p>
          </div>
          <a class="route-btn" href="https://map.naver.com/v5/search/%EC%84%9C%EC%9A%B8%EC%8B%9C%20%EB%A7%88%ED%8F%AC%EA%B5%AC%20%EB%8F%85%EB%A7%89%EB%A1%9C6%EA%B8%B8%208" target="_blank" rel="noopener noreferrer">
            <span>길 찾기</span>
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
              <polygon points="3 11 22 2 13 21 11 13 3 11"></polygon>
            </svg>
          </a>
        </div>
      </div>

      <!-- Right side: Classroom Image Carousel -->
      <div class="classroom-slider-wrapper">
        <div class="classroom-slider">
          {#each classroomSlides as slide, index}
            <div class="classroom-slide {index === currentClassroomSlide ? 'active' : ''}">
              <img src={slide.url} alt={slide.alt} />
            </div>
          {/each}

          <!-- Pagination dots -->
          <div class="dots-container">
            {#each classroomSlides as _, i}
              <button 
                class="dot {i === currentClassroomSlide ? 'active' : ''}" 
                onclick={() => selectClassroomSlide(i)}
                aria-label="{i + 1}번 실습실 전경 보기"
              ></button>
            {/each}
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<Certifications />


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
    width: 90px;
    height: 90px;
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
    margin-top: 10px;
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

  /* Apply Section (Section 4) */
  .apply-section {
    position: relative;
    padding: 90px 0;
    background-image: linear-gradient(rgba(0, 0, 0, 0.75), rgba(0, 0, 0, 0.75)), url('https://cdn.imweb.me/thumbnail/20230908/7647fcd6dbe29.png');
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

  .step-desc a {
    color: var(--primary);
    text-decoration: underline;
    font-weight: 500;
  }
  
  .step-desc a:hover {
    color: var(--primary-hover);
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
    color: var(--text-muted);
    line-height: 1.55;
    word-break: keep-all;
  }

  .payment-divider {
    height: 1px;
    background-color: var(--border-color);
    margin: 4px 0;
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

  /* Center Section (Section 5) */
  .center-grid {
    display: grid;
    grid-template-columns: 1.1fr 0.9fr;
    gap: 30px;
    margin-top: 10px;
  }

  .map-wrapper {
    display: flex;
    flex-direction: column;
    background-color: var(--bg-white);
    border-radius: 24px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.02);
    border: 1px solid rgba(0, 0, 0, 0.04);
    height: 440px;
  }

  .map-container {
    flex: 1;
    width: 100%;
    background-color: #ededed;
  }

  .address-bar-card {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 25px;
    background-color: var(--bg-white);
    border-top: 1px solid var(--border-color);
  }

  .address-text strong {
    font-size: 15px;
    font-weight: 700;
    color: var(--dark);
    display: block;
    margin-bottom: 4px;
  }

  .address-text p {
    font-size: 14px;
    color: var(--text-muted);
    margin: 0;
  }

  .route-btn {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background-color: var(--primary);
    color: #ffffff;
    padding: 10px 20px;
    border-radius: 30px;
    font-size: 14px;
    font-weight: 700;
    transition: all var(--transition-speed);
  }

  .route-btn:hover {
    background-color: var(--primary-hover);
    transform: translateY(-2px);
    box-shadow: 0 4px 10px rgba(255, 121, 0, 0.25);
  }

  /* Classroom Slider Wrapper */
  .classroom-slider-wrapper {
    position: relative;
    border-radius: 24px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.02);
    border: 1px solid rgba(0, 0, 0, 0.04);
    height: 440px;
  }

  .classroom-slider {
    position: relative;
    width: 100%;
    height: 100%;
  }

  .classroom-slide {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    opacity: 0;
    transition: opacity 0.8s ease-in-out;
    z-index: 1;
  }

  .classroom-slide.active {
    opacity: 1;
    z-index: 2;
  }

  .classroom-slide img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  /* Dot indicators at the bottom center of the classroom slider */
  .dots-container {
    position: absolute;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 8px;
    z-index: 10;
    background-color: rgba(0, 0, 0, 0.3);
    padding: 8px 12px;
    border-radius: 20px;
    backdrop-filter: blur(4px);
  }

  .dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background-color: rgba(255, 255, 255, 0.5);
    border: none;
    cursor: pointer;
    padding: 0;
    transition: all 0.3s ease;
  }

  .dot.active {
    background-color: #ffffff;
    transform: scale(1.25);
    width: 16px;
    border-radius: 4px;
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

    .center-grid {
      grid-template-columns: 1fr;
      max-width: 650px;
      margin: 0 auto;
      gap: 24px;
    }

    .map-wrapper {
      height: 380px;
    }

    .classroom-slider-wrapper {
      height: 320px;
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
      width: 75px;
      height: 75px;
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

    .map-wrapper {
      height: 320px;
    }

    .classroom-slider-wrapper {
      height: 240px;
    }
    
    .address-bar-card {
      padding: 15px;
      flex-direction: column;
      align-items: flex-start;
      gap: 12px;
    }

    .route-btn {
      width: 100%;
      justify-content: center;
    }
  }
</style>
